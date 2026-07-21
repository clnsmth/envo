# ENVO Agentic Curation Workflow: Testing and Verification Protocol

This document outlines a step-by-step manual testing protocol to validate the ENVO Agentic Ontology Curation Workflow. This workflow is defined in [.github/workflows/ai-agent.yml](.github/workflows/ai-agent.yml) and governed by the ontology curation guidelines in [AGENTS.md](AGENTS.md).

---

## 📋 Prerequisites & Test Setup

Before executing any tests, ensure the following configuration is active in the repository where the tests are run:

### 1. Authorized Controllers
The workflow limits triggering to users listed in [.github/ai-controllers.json](.github/ai-controllers.json).
* **Authorized User**: `clnsmth` (must match your GitHub username).
* **Unauthorized User**: Any other GitHub account (useful for negative/security testing).

### 2. Required GitHub Secrets
The following secrets must be present in the GitHub repository's Settings under **Secrets and variables > Actions**:
* `APP_ID`: GitHub App ID with read/write access to repository contents, issues, and pull requests.
* `APP_PRIVATE_KEY`: The private key file (`.pem`) for the GitHub App.
* `GEMINI_API_KEY`: API key for model executions.
* `ANTIGRAVITY_API_KEY`: API key for running the `antigravity-cli` tool.

---

## 🧪 Test Case 1: Workflow Triggering & Authorization Verification

**Goal**: Verify that mentioning the agent triggers the curation process only when performed by an authorized user.

### Action A: Triggering by an Authorized User
1. Log in to GitHub as `clnsmth`.
2. Navigate to your fork/repository and open a new **Issue**.
3. Title: `Test Triggering: Hello Agent`
4. Body:
   ```text
   @clnsmth-ontology-agent please tell me what your primary layout is.
   ```
5. Submit the issue.

#### Expected Outcomes:
* **Eyes Reaction**: Within 30 seconds, the agent should add an 👀 reaction to the issue description.
* **Initialization Comment**: The agent will post a comment indicating it is on the task:
  > 🤖 Working on it...  
  > Follow along: [View workflow run](https://github.com/.../actions/runs/...)
* **Successful Execution**: The workflow completes, and the agent responds to your comment with a summary of the layout defined in [AGENTS.md](AGENTS.md).

### Action B: Attempting Triggering by an Unauthorized User (Negative Test)
1. Log in to GitHub using a different account (one NOT in `.github/ai-controllers.json`).
2. Navigate to the same repository and open a new Issue, or comment on an existing one.
3. Title: `Test Triggering: Unauthorized Attempt`
4. Body:
   ```text
   @clnsmth-ontology-agent please check if seawater is present.
   ```
5. Submit the issue/comment.

#### Expected Outcomes:
* **No Reaction / Comment**: The agent does **not** add an 👀 reaction or post any initialization comment.
* **Early Exit**: In the GitHub Actions tab, the `check-mention` job will run, print `Allowed: false` in the logs, and the workflow will terminate successfully without initiating the expensive `respond-to-mention` curation job.

---

## 🧪 Test Case 2: Concept Existence Check & Existing Term Handling

**Goal**: Confirm that the workflow detects when a requested term already exists in ENVO and halts execution early after notifying the user.

### Steps:
1. Log in to GitHub as `clnsmth`.
2. Open a new **Issue**.
3. Title: `Curation Request: Add Sea water`
4. Body:
   ```text
   @clnsmth-ontology-agent please add the term 'sea water' as a subclass of 'environmental material' with definition 'A liquid environmental material consisting of water and dissolved salts'.
   ```
5. Submit the issue.

### Curation Agent Execution Flow (Behind the Scenes):
1. The GitHub Actions runner triggers `ai-agent.yml` in the `obolibrary/odkfull` container.
2. The agent compiles the local SemSQL SQLite database:
   ```bash
   cd src/envo && ./run.sh semsql make envo.db
   ```
3. The agent searches the compiled database for the proposed term `seawater`:
   ```bash
   runoak -i sqlite:src/envo/envo.db search 'seawater'
   ```
4. Since `seawater` (ID `ENVO:01000007`) already exists, the search returns a high-confidence exact match.

### Expected Outcomes:
* **Early-Exit Notification**: The agent posts a comment back to the issue referencing the duplicate:
  > 🤖 **Concept Already Exists**  
  > The term **seawater** is already present in the Environment Ontology.
  > * **Ontology ID**: `ENVO:01000007`
  > * **Definition**: Water containing dissolved salts, which is located in a sea or ocean.
  > * **No modifications were made.**
* **Cessation**: Curation stops. No git branch is created, no files are modified, and no pull requests are opened.

---

## 🧪 Test Case 3: New Term Creation (End-to-End ROBOT Pipeline)

**Goal**: Validate the entire curation pipeline—from duplicate check failure to ROBOT template compilation, local reasoner testing, code commit, and PR creation.

### Steps:
1. Log in to GitHub as `clnsmth`.
2. Open a new **Issue**.
3. Title: `Curation Request: Abyssal Bioluminescent Zone`
4. Body:
   ```text
   @clnsmth-ontology-agent please add a new term 'abyssal bioluminescent zone' as a subclass of 'marine biome'.

   Definition: A marine biome which is 1) located in the abyssal zone and 2) characterized by the presence of bioluminescent organisms.
   Created By: https://orcid.org/0000-0003-2261-9931
   Reference: https://en.wikipedia.org/wiki/Abyssal_zone
   ```
5. Submit the issue. Let's assume the issue number assigned by GitHub is `#100`.

### Curation Agent Execution Flow (Behind the Scenes):
1. **Trigger & Initialize**: Curation starts. The agent posts the working link.
2. **Duplicate Check**: The agent executes OAK search on `abyssal bioluminescent zone`. No matches are returned, verifying that the concept is absent.
3. **Hierarchy Search**: The agent checks the parent class `marine biome` (`ENVO:01000048`) to ensure it exists and is structurally valid.
4. **Git Branching**: The agent checks out a new branch:
   ```bash
   git checkout -b clnsmth-ontology-agent-issue-100-run1
   ```
5. **ROBOT Curation Template**: The agent writes a new curation row inside `src/envo/modules/temporary_robot_template.csv` using LF line endings. It pulls an unused ID from the active curator ID range (e.g. `ENVO:3620000` series) or its own reserved ID pool, mapping headers precisely:
   * **Ontology ID**: `ENVO:03620001` (for example)
   * **label**: `abyssal bioluminescent zone`
   * **parent class**: `marine biome`
   * **definition**: `A marine biome which is 1) located in the abyssal zone and 2) characterized by the presence of bioluminescent organisms.`
   * **definition cross reference**: `https://orcid.org/0000-0002-4366-3088`
   * **created by**: `https://orcid.org/0000-0002-4366-3088`
   * **creation date**: `2026-07-20T07:02:13Z`
   * **term_tracker_item**: `https://github.com/clnsmth/envo/issues/100`
6. **ROBOT Compilation**: The agent converts the template into an OWL module:
   ```bash
   robot template --template modules/temporary_robot_template.csv -i envo-edit.owl --prefix "RO:http://purl.obolibrary.org/obo/RO_" --prefix "ENVO:http://purl.obolibrary.org/obo/ENVO_"  --ontology-iri "http://purl.obolibrary.org/envo/modules/temporary_robot_template.owl" convert --format ofn -o modules/temporary_robot_template.owl
   ```
7. **ROBOT Merge**: The agent merges the module into the primary source file:
   ```bash
   robot merge --input envo-edit.owl --input modules/temporary_robot_template.owl --collapse-import-closure false convert --format ofn --output envo-edit.owl
   ```
8. **Syntactic & Logical Validation**: The agent runs the local ENVO verification tests to confirm there are no unsatisfiable classes or reasoner faults:
   ```bash
   cd src/envo && make test
   ```
9. **Commit & Push**: The agent stages and commits both `src/envo/envo-edit.owl` and `src/envo/modules/temporary_robot_template.csv`, signing with `@clnsmth-ontology-agent`.
10. **Pull Request Submission**: The agent opens a PR via `gh pr create` pointing back to the issue, tagging `@pbuttigieg` and `@cmungall` for review.

### Expected Outcomes:
* **Automatic Pull Request**: A new Pull Request is created on your repository with a clear title (e.g. `Curation: Add abyssal bioluminescent zone #100`).
* **PR Reviewers Assigned**: `@pbuttigieg` and `@cmungall` are tagged or requested as reviewers in a comment inside the PR.
* **Curation Closure Comment**: The agent comments on the original issue `#100`, linking directly to the created PR:
  > 🤖 **New Term Successfully Created**  
  > I have created the requested term and compiled it using the ROBOT pipeline!
  > * **Term**: `abyssal bioluminescent zone` (`ENVO:03620001`)
  > * **Parent**: `marine biome`
  > * **Pull Request**: [Link to created PR]
  > 
  > CC: @pbuttigieg, @cmungall
* **Git Commit Signatures**: Running `git log` on the created branch shows the commit is signed as `@clnsmth-ontology-agent`.
