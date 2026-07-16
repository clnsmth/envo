In order to encourage domain experts, data curators or other collaborators wanting to work with us to more efficiently add material to ENVO, we have created the following **robot template and merge** workflow. This workflow involves using a shared [google sheet](https://docs.google.com/spreadsheets/d/1K5GWxpSF2s397FMjdb9nGicC8xl10Xd1-M3-4fOnCts/edit?usp=sharing) as the input for a [Robot template](https://robot.obolibrary.org/template) in which ontologists and domain experts can collaboratively edit and revise a batch of new term requests prior to merging them into the ontology. This workflow could be modified for use in other OBO ontologies. For those interested in becoming regular ENVO contributors, please also consult our [contributor guidance](https://github.com/EnvironmentOntology/envo/wiki/Adding-classes-to-ENVO) page. 

# Table of Contents

## [Steps for collaborators](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#steps-for-collaborators-1) 

* [Create an issue](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#1-create-an-issue)

* [Create a template google sheet](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#2-create-a-template-google-sheet)

* [Prepare new terms](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#3-prepare-new-terms)

  * [Basics about Robot templates](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#basics-about-robot-templates)

  * [Ontology IDs](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#ontology-ids)

  * [Term labels](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#term-labels)

  * [Parent classes](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#parent-classes)

  * [Definitions](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#definitions)

  * [Comments](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#comments)

  * [Citations](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#citations) 

  * [Editors notes](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#editors-notes)

  * [Synonyms](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#synonyms)

  * [Subsets](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#subsets)

  * [Cross references](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#cross-references)

  * [Axioms](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#axioms)

  * [Creation date](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#creation-date)

  * [Created by](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#created-by)

  * [Columns with multiple entries](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#columns-with-multiple-entries)

  * [Adding to existing terms](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#adding-to-existing-terms)

  * [Example robot template](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#example-robot-template)



## [Steps for ontology engineers](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#steps-for-ontology-engineers-1)

* [Setup](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#1-setup)

* [Download the google sheet](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#2-download-the-google-sheet)

* [Create a new branch](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#3-create-a-new-branch)

* [Save as csv](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#4-save-as-csv)

* [Compile the robot template](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#5-compile-the-robot-template)

* [Run robot merge](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#6-run-robot-merge)

* [Manually address issues in editors note](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#7-manually-address-issues-in-editors-note)

* [Create a Pull Request](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#8-create-a-pull-request)

* [Additional Notes](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#additional-notes)


***



# Steps for collaborators


## 1. Create an issue 

If you are interested in submitting a batch of term requests, create a new [ENVO github issue](https://github.com/environmentontology/envo/issues). In the issue description, thoroughly document the scope and motivation for the new terms including references to appropriate literature or other materials. 

## 2. Create a template google sheet


Copy or save the contents of this [ENVO template google sheet](https://docs.google.com/spreadsheets/d/1K5GWxpSF2s397FMjdb9nGicC8xl10Xd1-M3-4fOnCts/edit?usp=sharing) as a **new google sheet**. Make sure to name, as well as share it. Giving appropriate permissions to all necessary parties. Also make sure to add the link to the new template google sheet in the github issue.

## 3. Prepare new terms

When submitting a batch of new term requests it is very helpful to the ontology developers if our term-submitting collaborators can do some of the preliminary work by writing out information such as `term labels`, `parent classes`, `definitions`, `comments`, `synonyms` and `citations`. The [template google sheet](https://docs.google.com/spreadsheets/d/1K5GWxpSF2s397FMjdb9nGicC8xl10Xd1-M3-4fOnCts/edit?usp=sharing) contains columns for these as well as other information. The remaining sections explain how and what to add when filling out a new template sheet. 

### Basics about Robot templates

Robot templates are structured as follows, the first row contains `headers`, these are human-readable labels that are not complied by robot. These can be renamed as needed. The second row contains the `templates` which must contain strings formatted as specified in the [template documentation](https://robot.obolibrary.org/template). Please do not change these. All subsequent rows are for the addition of new terms or the modification of existing ones. 

### Ontology IDs

In order for a term (a row in the spreadsheet) to be recognized by robot and compiled into owl code, it must have an entry in the `ontology ID` column. IDs must start with `ENVO:` and be followed by eight digits e.g., `ENVO:03000102`. This is refereed to as the `curie` ID formatting style. The ID must be within an appropriate numeric ID range. New collaborators can request to have their own range ID ranged added within the [envo-idranges.owl](https://github.com/EnvironmentOntology/envo/blob/master/src/envo/envo-idranges.owl) file. Double check that IDs used are either new and in a approved range, or correspond to existing classes to which you intend to add new properties. If you do not yet have an ID range or only want to add a few terms you can leave this column blank for new terms and one of the main editors can fill it in. 

### Term labels

In the `label` column, put the primary labels for new terms, e.g., `marsh` or `water ice`. ENVO follows lower case naming conventions, with exceptions for proper nouns, e.g., `Taylor column` or `WMO blizzard`. 

### Parent classes

If known, add a `parent class` relationship (determined while formulating a genus-differentia style definition, see definition section below). For example if you were adding a new class `ice fog`, you'd add the `parent class` `fog`. If you are not sure about what the `parent class` should be, just leave it blank or make a google sheet comment in the `parent class` column about what you think might be appropriate. You can simply type or copy paste the label of an existing term into the `parent class` column.

To discover ENVO terms, you can browse ENVO from [here](https://www.ebi.ac.uk/ols/ontologies/envo) in order to discover appropriate parent classes.

<img width="600" alt="image" src="https://user-images.githubusercontent.com/12255688/100492959-1959ab00-3100-11eb-87a2-a53454297d16.png">

For more information about how to browse ENVO, as well as its contents, please familiarize yourself with our [navigating ENVO](https://github.com/EnvironmentOntology/envo/wiki/Navigating-ENVO) wiki page. The navigation page describes how one can go about viewing ENVO, as well as what the main hierarchies are, giving examples of each.

 

### Definitions

Please thoroughly consult our [Creating good definitions](https://github.com/EnvironmentOntology/envo/wiki/Creating-good-definitions) page, paying close attention to the details described there in order to formulate definitions. Definitions are very important to the ENVO curation team, and are useful in determining basic parent/child (superclass/subclass) relationships between concepts. When writing definitions we follow a genus-differentia structure, i.e., `An A is a B which C's`. The genus refers the parent term, and the differentia is that which makes a term different (or more specific) than its parent. Where possible we encourage our collaborators to help us incorporate their term requests by formulating a first pass at a simple definition following this structure.

For example in ENVO, example we define [land ice mass](http://purl.obolibrary.org/obo/ENVO_01001547) as:

> An ice mass which has formed over land.

Doing this exercise helps both to formulate a concise and simple definition as well as determine the appropriate parent class. In cases where there are multiple differentiating factors, they can be written out as a list, for example:

> A blizzard which has 1) winds of 35 miles per hour (approximately 56 kilometers per hour), 2) lasts for a period of at least three hours, and 3) occurs in extremely cold temperatures. 
 
For more information about writing ontology definitions please see [this blog post](https://douroucouli.wordpress.com/2019/07/08/ontotip-write-simple-concise-clear-operational-textual-definitions/) written by Dr. Chris Mungall.

### Comments

In the `comment` column add any supporting information which is relevant to the term but not essential for its definition. As described in the above [blog post](https://douroucouli.wordpress.com/2019/07/08/ontotip-write-simple-concise-clear-operational-textual-definitions/), any information which gets cut out while trying to write a concise genus-differentia style definition can be put into the comment. 

### Citations

To link newly defined terms to scholarly or other references, please use the `definition cross reference` and `comment cross reference`, columns respectively. Note that these fields expect URLs e.g.,  `http://glossary.ametsoc.org/wiki/Main_Page`. If a reference is not a URL e.g., a book ISBN number, then add a comment in the editors note mentioning that particular reference (naming it) is not a URL. This way the engineers will know to change the field from an URL to a string. 

It is possible and encouraged to add multiple references into the `definition cross reference` or `comment cross reference` columns. You can do so by typing all the references in a single cell delimited by a `|` character without spaces, for example:

```
https://www.ec.gc.ca/meteo-weather/|http://www.bom.gov.au/lam/glossary/|http://glossary.ametsoc.org/wiki/Main_Page|http://www.physicalgeography.net/glossary.html
``` 

Note that when adding definition or comment references to an existing ENVO term which already has a definition or comment, you won't be able to use the `definition cross reference` and `comment cross reference` columns alone. You can either 1) add an editors note saying to add the following references onto the definition or comment, or 2) copy the exiting comment or definition into the google sheets `comment` or `definition` columns and add the new reference(s) in the `definition cross reference` and or `comment cross reference` columns, making an editors note explaining about what was done. Use option 1 if the existing definition or comment already has `definition cross reference` annotations on it within ENVO. 

### Editors notes

The `editors note` column serves as a place for ontology developers to make notes about how to better engineer the term or discuss future developments. For the most part this can be ignored, however, if you want to replace an existing terms definition please make a note here in the `editors note` column that a new definition was added. If one of the references provided was not a URL then please also note it here. 

### Synonyms

Note that the template contains four types of synonyms: `exact synonym`, `broad synonym`, `narrow synonym`, `related synonym`. These follow SKOS style (broader or narrow) conventions. Use `exact synonym` for interchangeable term labels, for example `seawater` and `sea water`. Use `broad synonym` when a synonym might refer to more than just the term of interest, for example the term `acid rainfall` has `acid rain` as a broad synonym because `acid rain` might refer to more than just the process of acid rainfall. When a synonym refers to something more specific than the class label use `narrow synonym`, for example `road` has narrow synonym `highway`. If you unsure about a synonym being broader or narrower then use `related synonym`.

### Subsets

The `in subset` column is a place to specify if a term should belong within a named `subset`, a small extract of the overall ontology pertinent to a specific user community, e.g. `envoPolar`. For more information see our [subsets wiki page](https://github.com/EnvironmentOntology/envo/wiki/Subsets).


### Cross references

The `cross reference` column provides a generic way to link to other resources such as mappings to other vocabularies. If a project leveraging this workflow is mapping ENVO terms to another resource, e.g. the Semantic Web for Earth and Environment Technology Ontology (SWEET), you could create a new `cross reference` column for that specific resource. Although you can name the column anything you'd like in the header row (first row), the template (2nd row) must be `AI oboInOwl:hasDbXref SPLIT=|`. Note that the `definition cross reference` and `comment cross reference` columns use the same `oboInOwl:hasDbXref`, those however are intended to serve as citation references for a terms definition and comment respectively, see the [citations](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#citations) section. 

### Axioms

The `subclass axiom` column is for the creation of machine-readable links between classes. An example of this is the ENVO term [ocean](http://purl.obolibrary.org/obo/ENVO_00000015) which has the axiom `composed primarily of some sea water`. If there are any relationships you'd like the ontologists to capture within axioms, make a google sheet comment in the `subclass axiom` column explaining in English to **what** and **how** the term might link to other terms. for example if the new term were `beach` you might say `A beach is part of a shore.`

Examples of properties commonly used in ENVO axioms include: `composed primarily of`, `part of`, `has part`, `overlaps`, `occurs in`, `input of`, `output of`, `formed as result of`, `located in`, `location of`, `occurs in`, `adjacent to`, `determined by`, and `has quality`. Note that within the ontology framework ENVO adheres to, there are strict rules about what types of properties can link what types of classes, however, please still feel free to suggest axioms when appropriate. The ontologists can figure out the details of how to correctly express the suggested relationships. 

### Creation date

To fill out the `creation date` column you can copy paste the output from [isotimestamp.com](https://www.isotimestamp.com/).

### Created by

In order to micro-credit collaborators and terms editors, ENVO includes links to [ORCIDs](https://orcid.org/). To fill out the `created by` column, put a `|` delimited list of the ORCIDs of everyone contributing to the term development process. For example: 

```
https://orcid.org/0000-0003-4808-4736|http://orcid.org/0000-0002-4366-3088|http://orcid.org/0000-0002-3410-4655|https://orcid.org/0000-0003-0632-7576|https://orcid.org/0000-0002-0337-8610|https://orcid.org/0000-0002-2282-7215
```

Make sure to use the full IRI for the ORCIDs e.g., `https://orcid.org/0000-0003-4808-4736` not just the digits at the end.

### Columns with multiple entries

Any columns with `SPLIT=|` at the end of the string in the template (second) row allow for multiple `|` delimited inputs where each of which is compiled separately. For example adding `boulbie|buran|purga` within the `narrow synonym` column, which has the template `AL oboInOwl:hasNarrowSynonym@en SPLIT=|` will produce three separate narrow synonym annotation properties: 

<img width="200" alt="image" src="https://user-images.githubusercontent.com/12255688/100488568-008bce00-30dd-11eb-8c83-45324df24f0f.png">



### Adding to existing terms

It is possible to add onto existing terms using this workflow. When doing so, however, make sure not to retype any existing information that you do not intend to modify. For example **do not** type the existing label into the `label` or `parent class` columns. Doing so will create a duplicate of the label (or parent class) which will need to be deleted later. Instead, put a google sheet comment in the slot showing what the label is. If you are requesting to change an existing terms definition, add the new one in the `definition` column and also add a short message in the `editors note` column saying that you'd like to update the existing definition.

### Example robot template

The following is an example row of a completed template. Note that subclass axioms can be complicated, hence we **do not** ask our collaborators to provide them. If desired one could provide suggestions for them in English (see the [Axioms](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow#axioms) section). 

Ontology   ID | label | parent   class | definition | definition   cross reference | comment | comment   cross reference | editors   note | exact   synonym | broad   synonym | narrow   synonym | related   synonym | in   subset | cross   reference | subclass   axiom | creation   date | created   by
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
ENVO:03000138 | grain   of snow | ice   mass | An   ice mass which is 1) on the order of 1 milimeter or less, 2) has a white and   opaque appearance, and 3) is formed as a result of water droplets freezing   during precipitation. | https://orcid.org/0000-0003-4808-4736 | Precipitation   in the form of very small, white opaque particles of ice; the solid   equivalent of drizzle. … | https://orcid.org/0000-0003-4808-4736 | Need   to add a computable size constraint. | snow   grain |   |   | granular   snow | envoPolar |   | ('has   quality' some (opaque and white))\|('formed as result of' some 'freezing of   water into water ice')\|('participates in' some 'precipitation process') | 2020-11-18T20:22:03.870Z | https://orcid.org/0000-0003-4808-4736\|http://orcid.org/0000-0002-4366-3088\|http://orcid.org/0000-0002-3410-4655\|https://orcid.org/0000-0003-0632-7576\|https://orcid.org/0000-0002-0337-8610\|https://orcid.org/0000-0002-2282-7215






***


# Steps for ontology engineers


### 1. Setup

1. Install [robot](https://robot.obolibrary.org/) following the `Getting Started` instructions.

2. clone ENVO `git clone https://github.com/EnvironmentOntology/envo.git`, (or use `git@github.com:EnvironmentOntology/envo.git` with ssh keys to skip writing out one's username/password). 



### 2. Download the google sheet

Once the domain experts and ontologists are satisfied with the new additions, download the google spreadsheet as a csv file:

<img width="400" alt="image" src="https://user-images.githubusercontent.com/12255688/100475205-86455480-30b0-11eb-9eb4-58ca42a5a04f.png">


### 3. Create a new branch

On the command line: 

1. Make sure you're up to date by running `git pull`

2. Checkout a branch corresponding to the new github issue, e.g., `git checkout -b issue-xyz`. Proceed with the remaining steps while on that branch. You can return to the main branch or back to this branch by running a command like `git checkout master`


### 4. Save as csv

Move or save the downloaded csv file to the `envo/src/envo/modules` directory saving the file as `temporary_robot_template.csv`. Make sure to save the file using `UTF-8` formatting. Using excel on a mac select it here:

<img width="300" alt="image" src="https://user-images.githubusercontent.com/12255688/99596404-31c11b80-29c4-11eb-83fa-62a16877bf73.png">


Note that some special characters, e.g., `°` or `ö` will not be saved properly during the google download process. If the template contains such characters, copy and paste the entire google template sheet into the local csv file (making sure to delete the contents of the latter prior to pasting in case the number of columns has changed). 

### 5. Compile the robot template

Now that the temporary robot template `csv` file is in place, you'll need to compile it into `owl` format by running the using the following command from `envo/src/envo`.
```
robot template --template modules/temporary_robot_template.csv -i envo-edit.owl --prefix "RO:http://purl.obolibrary.org/obo/RO_" --prefix "ENVO:http://purl.obolibrary.org/obo/ENVO_"  --ontology-iri "http://purl.obolibrary.org/envo/modules/temporary_robot_template.owl" convert --format ofn -o modules/temporary_robot_template.owl
```

This will create a new file `modules/temporary_robot_template.owl` you can examine it with protege to make sure it is as expected. Note that although this module imports terms used in axioms (such as superclasses), it only "knows" about labels of terms which have been defined in the template. For example here the template will only display `ENVO_01000903` not `blizzard`. Once merged this will no longer be an issue. 

<img width="159" alt="image" src="https://user-images.githubusercontent.com/12255688/100476863-b262d480-30b4-11eb-8f9e-2bccd2b0370a.png">


### 6. Run robot merge

Next, [merge](https://robot.obolibrary.org/merge) the newly generated owl file into the editors file: `envo-edit.owl`. 

```
robot merge --input envo-edit.owl --input modules/temporary_robot_template.owl --collapse-import-closure false convert --format ofn --output envo-edit.owl
```

Examine the changes made to `envo-edit.owl` by running a diff checker such as `git diff` or `git difftool` to make sure the only additions are those expected from the contents of the robot template. Once satisfied, commit the new changes by running a command such as the following. To that commit message you can optionally add a reference to the github issue e.g., `#xyz`.

```
git commit envo-edit.owl -m 'Run robot merge to add template terms to edit file'
```


### 7. Manually address issues in editors note

In a series of subsequent manual commits in protege, make sure to address all possible comments made in the editors note. Examples may include adding `database_cross_reference` annotations to existing definitions or comments, changing `database_cross_reference` from IRIs to strings (for books), or removing duplicated definitions or comments. 

A reasonable practice would be to make one commit message per term changed e.g., `git commit envo-edit.owl -m 'Fix dbxref for blizzard'`. Make sure to save manually added `database_cross_references` as IRIs (when appropriate):

<img width="450" alt="image" src="https://user-images.githubusercontent.com/12255688/100481767-2ad09200-30c3-11eb-91c6-53e1cd2e0cb6.png">


### 8. Create a Pull Request

Once the previous steps are completed you can run `git push`. Note that github on the command line will ask you to set origin master the first time to configure how to push correctly. After pushing a new branch return to the ENVO github page where it will ask you to create a pull request based on the recently pushed branch. 


### Additional Notes

**Keeping the branch up to date**

See this stack overflow post about [keeping-a-branch-up-to-date-with-master](https://stackoverflow.com/questions/19758915/keeping-a-branch-up-to-date-with-master). 

You can run: 
```
git fetch
git merge origin/master
```

**Adding class expressions with a robot template:**

Note that when adding other subclass axiom class expressions if there is more than a single term, the class expression will need to be wrapped in parenthesis. For example, in the `subclass axiom` column we can add: `('composed primarily of' some 'water ice')`. This can be prototyped in protege's `class expression editor` to make sure the axiom will compile correctly. Note that this stage of the workflow does **not** reason over the class expression to check for mistakes. Proceed with caution when adding object property class expressions via robot templates. These will, however, be checked during the Pull Request's travis build. Also note that the template won't compile if a class expression contains a term not referenced in `envo-edit.owl`. Ontologists may need to import terms as needed. 

<img width="300" alt="image" src="https://user-images.githubusercontent.com/12255688/100489107-5ada5e00-30e0-11eb-88f2-bb81ba1efc11.png">


**Reversing mistakes:**

If after committing, you realize that you've made a mistake you can always revert the branch back to a previous correct commit with a command like: `git revert --no-commit a206a333d507a6071bb93622163353fe4a46097c..HEAD` Run `git log` to find the hash id for the commit to revert back to. 

**Multiple definitions:**

Note that having multiple `definitions` will break the travis build process. Make sure to only have one per term when the pull request is submitted. 


**Robot commands:**

Note that the file created from the robot template and merge commands will be in [OWL Functional](http://www.w3.org/TR/owl2-syntax/) but will have `.owl` extensions. Note the following in the robot merge command 1) the `--collapse-import-closure false` flag will prevent direct imports from being merged in; 2) By default [robot merge](https://robot.obolibrary.org/merge) will only include the ontology annotations from the first input, `envo-edit.owl` thus preserving its annotations; 3) Like in the template command, [robot convert](https://robot.obolibrary.org/convert) is chained after the first (merge) command, allowing owl functional syntax to be used while maintaining a `.owl` file extension.

