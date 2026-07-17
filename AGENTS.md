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
*To be filled in: High-level principles for editing ENVO.*

## 5. Edits & Compilation
*To be filled in: Procedures for making edits, running ROBOT templates, and merging changes.*

## 6. Ontology Format Guidelines
*To be filled in: Standard naming conventions, ID structure (8 digits), and required term fields.*

## 7. Publications & Citations
*To be filled in: Citation rules, references, and fetching publication details.*

## 8. GitHub Contribution Process
*To be filled in: Branches, PR creation rules, commit message standards, and agent signatures.*

## 9. Handling GitHub Issues and Requests
*To be filled in: Tool usage with 'gh' to interact with issues and pull requests.*

## 10. Troubleshooting
*To be filled in: Validating OWL files and addressing compilation/reasoning errors.*

## 11. Obsoleting Terms
*To be filled in: De-axiomatizing and replacing obsolete concepts in ENVO.*

## 12. Other Metadata & Annotations
*To be filled in: Nano-crediting, dc:creator ORCIDs, and subset tagging.*

## 13. Relationships & Axioms
*To be filled in: Usage of relations (e.g., composed primarily of) and restrictions.*

## 14. Logical Definitions
*To be filled in: Genus-differentia structures and axiomatization styles.*
