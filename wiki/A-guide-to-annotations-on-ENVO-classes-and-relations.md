🚧 Work in Progress 🚧 
Authored by [Raïssa Meyer](https://orcid.org/0000-0002-2996-719X), reviewed by [PL Buttigieg](https://orcid.org/0000-0002-4366-3088)

# Table of Contents
* [What are annotation properties?](https://github.com/EnvironmentOntology/envo/wiki/A-guide-to-annotations-on-ENVO-classes-and-relations#what-are-annotation-properties)
* [Which annotation properties do we use in ENVO?](https://github.com/EnvironmentOntology/envo/wiki/A-guide-to-annotations-on-ENVO-classes-and-relations#which-annotation-properties-do-we-use-in-envo)
* [How do you add annotation properties to ENVO?](https://github.com/EnvironmentOntology/envo/wiki/A-guide-to-annotations-on-ENVO-classes-and-relations#how-do-you-add-annotation-properties-to-envo)


# What are annotation properties?

Very simply put, "annotation properties" are pretty much what they sound like: a helpful note that is added to an ontology class, instance, or relationship (or whole ontology itself), thus becoming a property of that class, relationship, etc. They are mostly meant to be human-readable, and do not affect the machine-actionable semantics of the term (from the point of view of a reasoner or other machine agent operating on the axioms in the ontology).

More technical specifications of what an annotation property is are available here:
* http://docs.oasis-open.org/plcs/dexlib/R1/dexlib/help/dex/techdes_owl_annotate.htm 
* https://www.w3.org/TR/owl-ref/#Header

Any classes added to ENVO will have annotation properties (APs), which typically reuse widely used and standardised qualifiers (e.g. from the rdf, rdfs, owl, dc, and similar vocabularies or other OBO ontologies, such as IAO). 

APs include the class label (`rdfs:label`), definition (`iao:definition`), sources consulted (`database_cross_reference`), additional comments (`rdfs:comment`), and many more. 

APs can either be added on the class level (e.g., `rdfs:label`, `iao:definition`) or as annotations on another annotation (e.g., `database_cross_reference` on an `iao:definition`).

# Which annotation properties do we use in ENVO?

In this section of the Wiki, we will go through the most important APs you will encounter when using or co-developing ENVO. 

## Essential annotation properties


#### `rdfs:label`
Each class in ENVO has a label (e.g., "piece of plastic"). Because they are human-friendly, labels are the first thing users would likely search for when looking for a class inside ENVO.

The class labels within ENVO should be unique (no two classes should have the same label). Because some terms are used loosely, or are ambiguous, you will notice that some labels inside ENVO are verbose. To make sure the more commonly used terms are associated with the class, we make frequent use of annotation properties that feature different kinds of synonymy (see below). For its labels, ENVO follows lowercase naming conventions, with exceptions for proper nouns, e.g., `Antarctic front`. 

#### `definition`
Each class in ENVO should have a human-readable definition. These definitions follow the genus/differentia format, i.e., `an A is a B which Cs`, where the genus (`B`) refers to the parent term, and the differentia (`which Cs`) is that which makes a term more specific than its parent and different from its siblings. Please consult the wiki page on [Creating good definitions](https://github.com/EnvironmentOntology/envo/wiki/Creating-good-definitions) for detailed guidance on how to write good definitions for ENVO. For additional perspectives, please also see this [blog post](https://douroucouli.wordpress.com/2019/07/08/ontotip-write-simple-concise-clear-operational-textual-definitions/) by Chris Mungall.

You may notice that some classes in ENVO lack a definition, or do not follow the genus/differentia format. We are continuously working on updating those to offer appropriate definitions. Please open an issue or a pull request if you can help spot and fix these (e.g. issue [#1159](https://github.com/EnvironmentOntology/envo/issues/1159)) In some cases, classes imported from other ontologies (e.g., PATO, GO, CHEBI, etc.) may not show the definitions in ENVO due to the minimal import process used. You can find all the APs on imported terms by following their URIs/IRIs.

## Optional but strongly recommended annotation properties

#### `dc:creator`
This Dublin Core AP is used to credit the person who created a class in ENVO. The AP usually includes the link to the [ORCID](https://orcid.org/) of the class creator, entered as an IRI rather than free text (e.g. by using the IRI Editor in Protege).

#### `dc:contributor`
This AP is used to credit the person or organisation who contributed to a class in ENVO. The AP usually includes links to the [ORCIDs](https://orcid.org/) of people who contributed to a class in ENVO.

#### `dc:date`
This Dublin Core AP is used to record the time and date a new class was added to ENVO. This is recorded in the ISO 8601 format `2021-05-21T07:28:07.093Z`. 

#### `database_cross_reference`

The `database_cross_reference` AP is from the oboInOwl collection of APs: `http://www.geneontology.org/formats/oboInOwl#hasDbXref`

This annotation may be used in different ways. Option (1) is used more often and may be more relevant to most users, while option (2) is used in more specific cases.

1) The AP may be used as an AP on other APs (e.g., on a definition, or comment) to reference material consulted when formulating content for the AP it adorns (e.g., sources for a definition).
   * This source may be given as a URL (preferred; e.g., https://cloudatlas.wmo.int/en/clouds-genera-altocumulus.html to cite the WMO cloud atlas definition), as a literal (e.g., a book ISBN number).
   * In certain cases, this AP can also refer to specific people, e.g., `MA:ma` (deprecated format) for Micheal Ashburner (who created most of ENVO's early classes), or links to [ORCIDs](https://orcid.org) (preferred format). 
2) The AP may be used on the class itself, in which case it would refer to the entire class. As an example, we're currently using that for mapping ENVO classes to other terminology resources. Please see the outcomes of [issue #1129](https://github.com/EnvironmentOntology/envo/issues/1129) for more information on existing mapping efforts. The mapped term may be referenced through
   * its term ID (e.g., `http://sweetontology.net/matrWater/SnowGrain` for mapping to the Snow Grain class in the Semantic Web for Earth and Environmental Technology (SWEET) Ontology).
   * a CURIE style code (e.g., EcoLexicon:abyssal_hill, FFT:1020 for a specific entry the Feature Type Thesaurus, or a generic pointer to a resource like ADL:FTT for the Alexandra Digital Library's Feature Type Thesaurus). These latter references are typically legacy annotations from ENVO's early days, and are being deprecated in favour of URIs or URLs if they are available.

The use of multiple `database_cross_reference` annotation properties is encouraged.

#### `editor note`

This AP is from the IAO: `http://purl.obolibrary.org/obo/IAO_0000116`

The editor note serves as a place for ontology developers to make notes about how to better engineer a class or note the need for future developments. It may note the need for further axiomatisation, the lack of a better parent class, etc. For an example, see the editor note on [cirrus cloud](https://www.ebi.ac.uk/ols/ontologies/envo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FENVO_01001160).

#### synonyms

The synonym APs may be used to 
* add an alternative class label in the same language as the class term.
* add multilingual interpretations of a class label (specified by the added language code, e.g., `de` for German, `it` for Italian, `zh` for Mandarin, etc.).

In ENVO, we make use of four types of synonyms: `has_broad_synonym`, `has_exact_synonym`, `has_narrow_synonym`, and `has_related_synonym`. 

These follow the Simple Knowledge Organisation System (SKOS) [logic for synonymy](https://www.w3.org/TR/skos-reference/), but - ***unlike the SKOS properties*** - they have literal values rather than IRIs/URIs to another SKOS:concept or OWL class. Thus, they use oboInOwl URIs 

##### `has_broad_synonym`
This AP is used when a synonym refers to more than just the term of interest, e.g., the class `acid rainfall` has `acid rain` as a broad synonym because `acid rain` might refer to either the process of acid rainfall or the rain itself. 

##### `has_exact_synonym`
This AP is used for interchangeable class labels, e.g., `seawater` and `sea water`.

##### `has_narrow_synonym`
This AP is used when a synonym refers to something more specific than the class label, e.g., `road` has_narrow_synonym `highway`. 

##### `has_related_synonym`
This AP is used when the class label you are adding as a synonym is neither exact, narrower, or broader, e.g., `ocean floor` has_related_synonym `sea floor`. These usually arise due to ambiguous linguistic usage of terms.

#### `in subset`

This AP (again from the oboInOwl collection) can be used to add a class in ENVO to an existing ENVO subset.

A subset is a slimmed down, but still logically sound version of the ontology. These are created for specific user communities or use cases. 

A class is added to a subset by specifying the name of the subset (e.g., `envoPlastics`) in this AP.

Please see our wiki page on [Making a new subset](https://github.com/EnvironmentOntology/envo/wiki/Making-a-new-subset) for more detail and to learn how to add a new subset, as well as [Subsets](https://github.com/EnvironmentOntology/envo/wiki/Subsets) to learn about existing subsets.

#### `rdfs:comment` 
This AP can be used to add notes about your class which e.g., are not necessarily true of all subclasses, or specify further details about parts of the definition which are not deemed as essential (and should thus not be added in the definition), but are still useful. 

For examples, note the definition and comment on [`cirrus cloud`](https://www.ebi.ac.uk/ols/ontologies/envo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FENVO_01001160) or on [microplastic particle](https://www.ebi.ac.uk/ols/ontologies/envo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FENVO_01000944).

## Further optional annotation properties

⏳ **NOTE: To be added**

# How do you add annotation properties to ENVO?

⏳ **NOTE: pictures to be added.**

As mentioned above, APs may be added on the class level or as APs on other APs. Below we will show how to use Protégé to add APs.

This guide assumes that you have already consulted the wiki page on [Adding classes to ENVO](https://github.com/EnvironmentOntology/envo/wiki/Adding-classes-to-ENVO), which explains the first steps for contributing to ENVO.

## Adding annotation properties on the class level

Adding class-level APs will be relevant for adding most of the APs introduced above (except for e.g., `database_cross_reference`, see section below).

The `rdfs:label` AP is automatically set up when you add a new class (it is the `Name` that you enter as part of that process).

If you have successfully configured Protégé for nano-crediting (see [here](https://github.com/EnvironmentOntology/envo/wiki/Adding-classes-to-ENVO#nano-crediting)), your ORCID (as `dc:creator`) and the time stamp (as `dc:date`) will automatically be added when you create a new class.

From there on, you may add further APs by 
1. clicking the `Annotations (+)` button, which you can find in the top right panel of the Protégé window on the top left side. This will open a pop-up window with all AP options. 
2. You'll have to find the type of AP you would like to add in the left panel of the pop-up window (scroll through) and click on it.
3. Then, select the appropriate addition type (`Literal` for literal text, `IRI editor` for URLs).
4. Enter the value (e.g. 'needs review by a domain expert') you wish to associate with the key (e.g. `editor note`) in the right panel of the pop-up window). Note that free text should be entered in the "Literal" tab of that panel, but URIs/IRIs/URLs should be entered in the "IRI Editor" tab.
5. Below the Literal input field, you'll see the option to specify the `Type` or `Lang` (language). In there, select or enter the language if needed (following the ISO 639-1 Language code, e.g., `en` for English, `de` for German, `zh` for Mandarin).
6. To save the changes made, click `OK` (which will close the pop-up window) and add your AP to the class. Your new addition should now show up in the top-right panel of the Protégé interface.

To further edit the AP, you can click on the `(O)` icon on that AP (which will show "Edit" if you hover over it), make your edits as described above, and click `OK` to save it.

To delete an existing AP, you can click on the `(X)` icon on that AP (which will show "Remove entity annotation" if you hover over it) and click `OK` to save it.


## Adding annotation properties on another annotation property

To add an AP on an already existing AP (e.g., for adding the source of a definition, or a comment on a specific AP, rather than the class itself), 
1. click on the `(@)` icon on the existing AP (which will show "Annotations" if you hover over it). This will open a pop-up window giving the class label of the class you are adding to, and the dc:creator (if given), as well as an `Annotations (+)` button. 
2. If you click on the `(+)` icon it will open the same pop-up window as we have seen when adding annotations on the class level. 
3. Again, you'll have to find the type of AP you would like to add in the left panel of the pop-up window, click on it, select the appropriate type (`Literal` for literal text, `IRI editor` for links and URIs), and enter your annotation. For free-text, select or enter the language if needed (following the ISO 639-1 Language code, e.g., `en` for English, `de` for German, `zh` for Mandarin).
4. If you are done click `OK` to close the big window. 
5. You can either continue to add additional APs now, by following the same steps again, or click `OK` to close the small window as well and hit `save` to save the changes you have made. Your new addition should now show up in the top-right panel of the Protégé interface.

To edit an AP on an AP, you can follow step 1. above, and then click on the `(O)` icon on the AP that you'd like to edit, make your edits as described above, and click `OK` and `OK` to save it.

To delete an AP on an AP, you can follow step 1. above, and then click on the `(X)` icon and save your edit.