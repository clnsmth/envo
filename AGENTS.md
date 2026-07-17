# ENVO Ontology Project Guide for AI Agents

This guide provides instructions and conventions for AI agents (such as Google Antigravity CLI) to edit, query, and contribute to the Environment Ontology (ENVO).

*Note: This guide is being built out iteratively. Details for each section will be populated in subsequent passes.*

## 1. Project Layout
- **Primary Development File**: `src/envo/envo-edit.owl`
  - All manual or automated ontology edits must be made directly in this file or via ROBOT templates in the modules directory.
  - ⚠️ **CRITICAL RESTRICTION**: Never edit release/compiled files directly (such as `envo.owl`, `envo.obo`, or `envo.json` in either the repository root or the `src/envo/` directory). These are derived files generated during the release build.
- **Assigned ID Ranges**: `src/envo/envo-idranges.owl`
  - This file stores the designated numeric ID ranges assigned to active curators and editors to prevent ID collisions. AI agents must strictly use IDs allocated within their permitted range.
- **ROBOT & Pattern Modules**: `src/envo/modules/`
  - Contains CSV templates and modules used by the ROBOT tool for automated term generation and bulk imports.
- **Local XML Catalog**: `src/envo/catalog-v001.xml`
  - Defines local URIs and mappings for resolving imported ontologies without requiring active internet connectivity.
- **Curation & Process Documentation**: `wiki/`
  - Houses comprehensive, up-to-date guides on annotations, relations, term creation, and ROBOT workflows.


## 2. Querying the Ontology
 We use the Ontology Access Kit (OAK) CLI (`runoak`) to query our local, dynamically generated SemSQL SQLite database instead of grepping raw files.           
                                                                                                                                                                  
- The compiled local SQLite database is located at: `src/envo/envo.db`                                                                                        
  - *(Note: If this file is missing in your local workspace, generate it on-the-fly by running `cd src/envo && ./run.sh semsql make envo.db`)*                
- To look up a specific term by ID:                                                                                                                           
    - `runoak -i sqlite:src/envo/envo.db info ENVO:00000114`                                                                                                  
- To search for terms matching a label or description (handles fuzzy matching & synonyms):                                                                    
    - `runoak -i sqlite:src/envo/envo.db search 'marine environment'`                                                                                         
- To find transitive parent/ancestor classes of a term:                                                                                                       
    - `runoak -i sqlite:src/envo/envo.db ancestors ENVO:00000114`                                                                                             
- Note that `runoak` is in your PATH, and `oaklib` is pre-installed in your environment.                                                                      
- DO NOT attempt to run standard grep or raw text searches over files in `src/envo/`; they are structured in OWL XML, and text-based grep is slow and highly  
inaccurate. Always use `runoak` against the SQLite DB.                                                                                                          
- ONLY use the methods above for searching the ontology.  

## 3. Before Making Edits
Before starting any ontology edits, agents must perform a series of semantic checks and research steps to maintain the high logical integrity of ENVO.

- **1. Read the Request & Gather Context**:
  - Carefully read the instruction or issue. If a GitHub issue is mentioned, view its detailed thread and discussion:
    ```bash
    gh issue view <issue-number>
    ```
  - If a literature reference (such as a PMID, DOI, or URL) is provided, ALWAYS fetch and read its abstract or full text to ensure definitions are accurate and scientific.

- **2. Check for Duplicates**:
  - NEVER create a term without first confirming that it does not already exist in the ontology (either as a primary label or an exact synonym).
  - Use the local `runoak` search to scan the SemSQL database:
    ```bash
    runoak -i sqlite:src/envo/envo.db search 'your term name'
    ```

- **3. Verify Parent Classes**:
  - Always check the proposed parent terms for logical consistency and scientific accuracy.
  - Run the `ancestors` command on candidate parent terms to inspect their position in the upper-level hierarchy:
    ```bash
    runoak -i sqlite:src/envo/envo.db ancestors ENVO:NNNNNNNN
    ```

- **4. Match Existing Design Patterns**:
  - Check `src/envo/patterns/` for any existing Dead Simple OWL Design Patterns (DOSDP) YAML templates that might apply to your term (e.g., `biome.yaml`, `ecosystem.yaml`, `atmospheric_material_subtype.yaml`).
  - If a relevant pattern is found, any new classes or assertions must strictly follow the schema and logical definitions specified in that pattern.


## 4. Editors Guide
- Design patterns are in `src/envo/patterns/`


## 5. Edits & Compilation
In ENVO, editing is strictly performed using the ROBOT template pipeline (CSV-based curation). We do not use Protégé. All template compilation, merging, and testing must be executed from `/src/envo/`.

### 1. Git Curation Workflow (ROBOT-based)
Always isolate your changes in a dedicated git branch matching the issue number.
1. **Synchronize upstream**:
   ```bash
   git pull
   ```
2. **Checkout a branch**:
   ```bash
   git checkout -b issue-xyz
   ```
3. **Prepare the CSV Template**:
   - Create or edit a CSV template file in `src/envo/modules/` (e.g., `temporary_robot_template.csv`).
   - Line endings must be **LF (not CRLF)** and special characters must be properly UTF-8 encoded.
4. **Compile the Template**:
   - Convert the CSV template into a temporary OWL module:
   ```bash
   robot template --template modules/temporary_robot_template.csv -i envo-edit.owl --prefix "RO:http://purl.obolibrary.org/obo/RO_" --prefix "ENVO:http://purl.obolibrary.org/obo/ENVO_"  --ontology-iri "http://purl.obolibrary.org/envo/modules/temporary_robot_template.owl" convert --format ofn -o modules/temporary_robot_template.owl
   ```
5. **Merge into the Edit File**:
   - Merge the OWL template back into the primary development file (`envo-edit.owl`):
   ```bash
   robot merge --input envo-edit.owl --input modules/temporary_robot_template.owl --collapse-import-closure false convert --format ofn --output envo-edit.owl
   ```
6. **Compile & Run Local Tests**:
   - Run the validation and build tasks inside `/src/envo/` to invoke the reasoner and check for logical inconsistencies (any unsatisfiable classes reasoning under `owl:Nothing` will fail the test):
   ```bash
   make test
   ```
7. **Commit & Push**:
   - Commit the updated `envo-edit.owl` along with the source CSV template, and push to create a PR:
   ```bash
   git add src/envo/envo-edit.owl src/envo/modules/temporary_robot_template.csv
   git commit -m "Run robot merge to add template terms #xyz"
   git push origin issue-xyz
   ```

### 2. Defining Relationship Axioms in ROBOT Templates
To add logical links between classes in ROBOT templates, use the `subclass axiom` column:
- **Class Expressions**: Class expressions containing multiple terms must be wrapped in parentheses.
  - *Format*: `('property' some 'value')`
- **Main Constituent Relation**: When describing the main constituent of an environmental material (`ENVO_00010483`), use the `'composed primarily of'` (`RO_0002473`) relation to map to other material entities or ChEBI chemical entities:
  - *Example subclass axiom cell*: `('composed primarily of' some 'water ice')` or `('composed primarily of' some 'water')` (where `water` is `CHEBI_15377`).

### 3. Compilation & Local Testing Commands
Execute these verification targets inside the `src/envo/` directory:
- **To run standard syntax, profile, and reasoner tests**:
  ```bash
  cd src/envo
  make test
  ```
- **To run full CI checks locally**:
  ```bash
  cd src/envo
  make continuous_integration_test
  ```


## 6. Ontology Format Guidelines
To maintain quality and logical consistency across all curated concepts, ENVO enforces strict structural and stylistic rules for term creation.

- **Term ID Format (8-Digit Curie vs. OWL Underscore)**:
  - ENVO IDs use **exactly 8 digits** (unlike 7-digit IDs common in other OBO ontologies).
  - **In ROBOT CSV Templates & OAK Queries**: Use the **colon format** (e.g., `ENVO:01001234` or `ENVO:03000102`). This is required in ROBOT template ID/parent columns and for running `runoak` commands.
  - **In Raw OWL Files (`envo-edit.owl` / `envo-idranges.owl`)**: The colon is replaced by an **underscore** (e.g., `ENVO_01001234`), as part of full PURL IRIs: `http://purl.obolibrary.org/obo/ENVO_01001234`.
  - **New Term Requests (NTRs)**: Assign IDs strictly within your assigned range in `src/envo/envo-idranges.owl` to avoid ID collisions. Never guess IDs; use `runoak` to verify.

- **Class Labels**:
  - Class labels are lowercase (e.g., `marsh`, `water ice`, `seawater`).
  - Proper nouns and standard groupings can be capitalized (e.g., `Taylor column`, `WMO blizzard`).

- **Textual Definitions (Genus/Differentia Form)**:
  - All definitions must follow OBO Foundry principles:
    - `A B which Cs` (e.g., `"A chair which is red"`)
    - `A B during which C` (e.g., `"A mass wasting process during which slab snow rapidly moves..."`)
  - **Exact Parent Match**: `B` must match the exact label of the direct parent class. Do not modify `B` with adjectives in the genus part (e.g. use `"A B which is green..."` instead of `"A green B..."`).
  - **Modular Numbered Lists**: If there are multiple differentiating attributes, use a clean numbered list:
    - *Format*: `A B which 1) C1s, 2) C2s, and 3) C3s`
  - **Universality**: Keep differentiae minimal and universally true of all subclasses of the term.

- **Definition Citations**:
  - Every definition must cite a reference (scholarly URL, PMID, DOI, or expert ORCID).
  - **Verbatim definition**: Map to the `definition source` (`IAO_0000119`) property.
  - **Paraphrased/Adapted definition**: Map to the `database_cross_reference` (`oboInOwl:hasDbXref`) property.


## 7. Publications & Citations
All term definitions and significant comments in ENVO must be substantiated with reliable literature or expert citations.

- **Finding References**:
  - Always search literature databases or the web to locate the correct PMIDs, DOIs, or persistent glossary URLs. Do not guess citations.
- **Reference Formats**:
  - **URLs/IRIs**: Strongly preferred for web glossaries and open databases (e.g., `https://cloudatlas.wmo.int/...`).
  - **PMIDs / DOIs**: Preferred for scholarly publications.
  - **ORCIDs**: Used to credit expert individual knowledge (e.g., `https://orcid.org/0000-0002-4366-3088`).
  - **Strings**: Literals like book ISBNs can be used if no persistent URI exists.
- **Specifying Citations in ROBOT Templates**:
  - Map verbatim text to `definition source` (`IAO_0000119`).
  - Map paraphrased/adapted text to `database_cross_reference` (`oboInOwl:hasDbXref`).
  - **Multiple References**: Combine multiple citations in a single ROBOT template cell using the pipe (`|`) delimiter without spaces:
    - *Example*: `https://www.ec.gc.ca/meteo-weather/|http://glossary.ametsoc.org/wiki/Main_Page`


## 8. GitHub Contribution Process
- most requests from users should follow one of two patterns:
    - you are not confident how to proceed, in which case end with asking a clarifying question (via `gh`)
    - you are confident how to proceed, you make changes, commit on a branch, and open a PR for the user to review
- Check existing terms before adding new ones
- For new terms: provide name, definition, place in hierarchy, and references
- Include PMIDs, DOIs, or URLs for all assertions
- Follow naming conventions from parent terms
- always commit in a branch, e.g. issue-NNN
- if there is an existing PR which you started then checkout that branch and continue, rather than starting a new PR (unless you explicitly want to abandon the original PR, e.g. it was on completely the wrong tracks)
- always make clear detailed commit messages, saying what you did and why
- always sign your commits `@clnsmth-ontology-agent`
- create PRs using `gh pr create ...`
- File PRs with clear descriptions, and sign your PR


## 9. Handling GitHub Issues and Requests
- Use `gh` to read and write issues/PRs
- Sign all commits and PRs as `@clnsmth-ontology-agent`


## 10. Troubleshooting
- **ROBOT Template Compilation Errors**:
  - If the CSV template fails to compile, append the `-vvv` verbose flag to the `robot template` command to view the full Java stack trace and pinpoint the exact row or cell formatting error:
    ```bash
    robot template -vvv --template modules/temporary_robot_template.csv -i envo-edit.owl ...
    ```
- **Reasoner or Profile Validation Failures**:
  - If `make test` fails, check the console output to identify unsatisfiable classes or syntax profile violations.
  - To trace low-level parsing issues in the main OWL file, you can run:
    ```bash
    robot convert -vvv -i envo-edit.owl -f ofn -o /dev/null
    ```


## 11. Other Metadata & Annotations
- Link back to the issue you are dealing with using the `term_tracker_item`
- All terms should have definitions, with at least one definition xref, ideally a PMID, DOI, or URL
- Sign terms with the nano-credited individual's ORCID using the `created_by` (or `dc:creator`) property


## 12. Relationships & Axioms
- All terms should have at least one parent (direct superclass), which is specified in the `parent class` column of ROBOT templates (or as an asserted subclass).
- Many terms in this ontology have relationships such as `part of` (`BFO_0000050`) or `'composed primarily of'` (`RO_0002473`).


## 13. Logical Definitions
- Logical definitions must follow the genus-differentia form, and the text definition should closely mirror the logical assertions (subclass axioms/restrictions).
- **Subclass Axioms in ROBOT Templates**:
  - Specify subclass restrictions in the `subclass axiom` column of the CSV.
  - Always wrap expressions containing multiple terms in parentheses: `('property' some 'value')`.
  - Use the pipe (`|`) delimiter without spaces to list multiple axioms in a single template cell.
- **Example**:
  - **Label**: `grain of snow`
  - **Parent Class**: `ice mass`
  - **Textual Definition**: `"An ice mass which is 1) on the order of 1 millimeter or less, 2) has a white and opaque appearance, and 3) is formed as a result of water droplets freezing during precipitation."`
  - **Subclass Axioms cell**: `('has quality' some (opaque and white))|('formed as result of' some 'freezing of water into water ice')|('participates in' some 'precipitation process')`


