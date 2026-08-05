# Skill: Mapping EDI Annotation Studio Term Requests to ENVO Schema

This skill defines the workflow and mapping rules for converting a "New term request received by the EDI annotation studio" into a standard "ENVO new term request" format.

## Triggering Condition
This workflow is triggered when a human curator mentions the agent on a GitHub issue containing an EDI-formatted term request, asking using any natural variation of:
`@clnsmth-ontology-agent please map this to the ENVO new term request` or `@clnsmth-ontology-agent please map this`.

The agent should recognize semantic variations such as:
- "map this"
- "convert to ENVO request"
- "translate this to ENVO schema"
- "format for ENVO"

## Input Format (EDI Annotation Studio Request)
The issue body typically contains the following markdown structure:
```markdown
# Proposed Ontology Term

### Suggested Term Name
[Suggested Term Name]

### Description
[Textual Description]

### Reference / Source
[Reference URL, PMID, or DOI]

### GitHub Username
@[Username]

### ORCID URL
[ORCID URL]

### Attribution Consent
[Attribution Consent Statement]
```

## Mapping & Validation Workflow

### Step 1: Extract Fields and Fill Blanks
Carefully parse and extract fields from the issue body:
- `SUGGESTED_NAME`: Extract from `### Suggested Term Name`
- `DESCRIPTION`: Extract from `### Description`
- `REFERENCE`: Extract from `### Reference / Source`
- `CREATOR_ORCID`: Extract from `### ORCID URL`
- `CREATOR_GITHUB`: Extract from `### GitHub Username`

**Handling Missing Information**:
If any required information is missing, the agent MUST do its best to fill in the blanks proactively:
1. **Missing Name**: Halt and request clarification.
2. **Missing Description**: Attempt to query Wikipedia or search the web using the term name to draft a plausible definition.
3. **Missing Reference**: Search PubMed, DOIs, or the web for scientific/standard definitions or articles related to the term and supply a valid reference URL.
4. **Missing ORCID**: Look up the GitHub user's public profile or use a placeholder, clearly flagging that the ORCID must be updated by the curator.
5. **Always flag any filled-in blanks** at the end of the post so curators are aware of what was inferred.

### Step 2: Check for Existing Terms (Duplicate Check)
Leverage existing local OAK search strategies:
```bash
runoak -i sqlite:src/envo/envo.db search "<SUGGESTED_NAME>"
```
Also search for alternative phrasings and lowercase variations.

- **IF EXACT MATCH IS FOUND**:
  Stop and report back immediately by posting a comment to the issue thread:
  ```markdown
  🤖 **Concept Already Exists**
  
  The term **<SUGGESTED_NAME>** already exists in the Environment Ontology.
  - **Ontology ID**: `ENVO:XXXXXXX` (replace with actual ID)
  - **Label**: `[Matched Term Label]`
  - **Definition**: `[Matched Term Definition]`
  
  No further mapping was performed.
  ```

### Step 3: Suggest Parent Classes
If the term does NOT exist, search ENVO for potential parent terms using OAK search to provide the curator with candidates:
```bash
runoak -i sqlite:src/envo/envo.db search "<keyword from SUGGESTED_NAME or DESCRIPTION>"
```
Identify 1-3 candidate parent classes and their IDs.

### Step 4: Formulate the OBO-Compliant Definition
Formulate a first-pass definition in the standard ENVO genus-differentia format:
- **Format**: `A [Suggested Parent Class] which [Description's differentiating features].`
- Follow these guidelines:
  1. Ensure the parent class is an actual, lowercase ENVO class label.
  2. Correct minor spelling/grammatical issues in the description (e.g. mapping "mater" to "matter").
  3. Ensure it starts with the standard indefinite article (`A` or `An`).

### Step 5: Post copy-pasteable ENVO New Term Request
Post a beautifully formatted markdown comment containing the final ENVO issue template format. This must be formatted exactly as a standard upstream ENVO new term request, so curators can copy and paste it directly:

```markdown
🤖 **Mapped ENVO New Term Request**

I have successfully mapped the EDI annotation studio term request to the standard ENVO new term request schema! 

You can copy and paste the markdown block below directly into a new issue on the upstream official ENVO ontology repository:

```markdown
# New Term Request: <SUGGESTED_NAME in lowercase>

- **Preferred Term Label**: <SUGGESTED_NAME in lowercase>
- **Textual Definition**: A [Suggested Parent Class] which [differentia based on Description].
- **Parent Class (Position in Hierarchy)**: [Suggested Parent Class] ([ENVO ID])
- **Definition Source / Reference**: <REFERENCE>
- **Exact Synonym(s)**: [Optional - list exact synonyms if any]
- **Created By**: <CREATOR_ORCID>
- **Creation Date**: <CURRENT_ISO_8601_TIMESTAMP>
- **Term Tracker Item**: <ISSUE_URL>
```

---
*Note on filled-in/inferred information:*
- [List any fields that were missing and filled/inferred by the agent, or 'None' if all were present in the source]
```
