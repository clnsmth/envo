🚧 ** UNDER CONSTRUCTION **🚧 

# Preamble

To maintain consistency and clarity of textual definitions, ENVO aligns to the [OBO Foundry & Library's sixth principle](http://obofoundry.org/principles/fp-006-textual-definitions.html). 

Clear textual definitions are the heart of a good semantic resource: they are what determines where a class (term) is placed in the ontology and how it connects to other classes. Further, they are the basis upon which machine-readable semantics (axioms, rule sets, etc) can be created and revised, as technologies and innovations enhance the computable expressivity of a semantic resource. 

When composing new content for ENVO, please follow the guidance below as closely as possible. If this guidance cannot be followed, reasons why should be recorded in the `rdfs:comment` field of each class (term) submitted or requested on our issue tracker.

We're aware of quite a lot of legacy content already in ENVO that doesn't follow the guidance below, but we're trying to revise that for consistency and to make sure new content is in better shape. 

# Basic guidance 

## Definition form

We use the genus/differentia form for definitions. The "genus" is the superclass of your class (the parent term of your term) and the "[differentia](https://www.merriam-webster.com/dictionary/differentia)" is the part where you can list the attributes of your term that set it apart from other subclasses of the genus. Thus:

All definitions MUST be of one of the following forms:
 * A `B` which `Cs`
   * e.g. `red chair =def. "A chair which is red"`
 * A `B` during which `C`
   * e.g. `slab avalanche =def. "A mass wasting process during which slab snow rapidly moves down a sloping surface"`

Please try to keep the differentiae minimal, noting that they MUST be true for all subclasses of your class. That is, if you state 'X =def. A B which is green`, all subclasses/subtypes of X MUST be green. Try to test your definitions by thinking of counterexamples. 

If there are properties or attributes of your class that aren't necessarily true of all subclasses, but are still useful, consider adding notes on those to an `rdfs:comment`. For example, note the definition and comment on [`glacier`](http://purl.obolibrary.org/obo/ENVO_00000133).


Further:
* `B` MUST be the exact term label of the superclass 
 * Caveat: we note that labels can change, however, please use the exact label at the time of creation. Changes are thus more easily propagated.
* `B` SHOULD NOT be modified with an adjective or similar qualifier, other than those in the `C` part of the definition.
  * Bad form: `A green B which Cs`
  * Good form `A B which is 1) green and 2) Cs`
* Differentiae should be concise and modular, try to break them down into lists rather than using complex grammar.
  * For example, if there are three differentiae, a definition SHOULD look like: `A B which 1) C1s, 2) C2s, and 3) C3s`
  * Example:  `cirrus castellanus cloud =def. "A cirrus cloud which 1) has linearly arranged cumuliform protuberances which resemble crenellated turrets or towers, some of which are taller than they are wide in some portion of their upper parts, 2) possesses a lower region or base which connects these protuberances."`
  * If you have many such differentiae in one definition, it may be sign that you should create some intermediate classes: 
    * e.g. Rather than `X =def. "A B which 1) C1s, 2) C2s, and 3) C3s"`, you may consider:
      * `Y =def. A B which C1s`
      * `Z =def. A Y which C2s`
      * `X =def. A Z which C3s`

## Finding superclasses

Finding out where to place a new class can be tricky, especially with large ontologies. We have some general advice, below, but please feel free to post a question on our issue tracker if in doubt. 
* Use an ontology search and browse interface (e.g. the ENVO interfaces on the [EBI OLS](https://www.ebi.ac.uk/ols/ontologies/envo) or [OntoBee](http://www.ontobee.org/ontology/ENVO)) to search for terms conceptually similar to yours (i.e. siblings terms) and look for their parents as possible superclasses.
* When in doubt, you could use more general terms (i.e. higher up in the hierarchy) as superclasses, but SHOULD try to find something as specific as possible. 
  * In ENVO, most terms would - at their most general - be some kind of [`environmental material`](http://purl.obolibrary.org/obo/ENVO_00010483), [`environmental system`](http://purl.obolibrary.org/obo/ENVO_01000254), [`ecosystem`](http://purl.obolibrary.org/obo/ENVO_01001110) or [`astronomical body part`](http://purl.obolibrary.org/obo/ENVO_01000813) (or - if you're dealing with space systems - [`astronomical body`](http://purl.obolibrary.org/obo/ENVO_01000799) or [`astronomical object`](http://purl.obolibrary.org/obo/ENVO_01000804)) 
* In general, you SHOULD NOT need to use classes from upper-level ontologies as superclasses. If you find yourself using upper-level ontology terms (e.g. BFO terms like [`material entity`](http://www.ontobee.org/ontology/ENVO?iri=http://purl.obolibrary.org/obo/BFO_0000040)), it's probably best to post a question on the tracker. 
* Somewhat advanced: look for equivalence axioms in the superclasses and see if your term can be reasoned into that branch. 
Give the manufactured product example. 

## Citations 

* All definitions MUST include URLs, URIs, or IRIs to references which were used to generate the definition.
  * If requesting a new term, definition citations MUST be stated in the corresponding issue.
  * If submitting a pull request please add the following annotation properties on the [definition](http://purl.obolibrary.org/obo/IAO_0000115) annotation property:
    * If the definition is not verbatim, these references SHOULD be added as [database cross-reference](http://www.geneontology.org/formats/oboInOwl#hasDbXref) annotation properties. 
    * If the definition is verbatim, it MUST be cited using the IAO [definition source](http://purl.obolibrary.org/obo/IAO_0000119) annotation property 
* Citations can also be ORCIDs or other forms of persistent individual identifiers if human expert knowledge was used. 
  * In this case, the submitter MUST be able to produce written consent from all individuals referenced to use their ORCID and contributed knowledge in such a manner. 


## Nanocrediting

Each contributed term SHOULD be nano-credited, so we can trace who contributed to and shaped the definition content. Thus:
* Each class SHOULD have at least one [dc:creator](http://purl.org/dc/elements/1.1/creator) annotation property (or textual field in a term request on our issue tracker) containing an ORCID or other persistent individual identifier.
* Additional contributors SHOULD be identified by one or more [dc:contributor](http://purl.org/dc/elements/1.1/contributor) annotation properties or textual field(s) in a term request on our issue tracker. 


# Advanced guidance

## Matching textual and axiomatic definitions 

Where possible, textual definitions SHOULD match those in the asserted subclass or equivalence axioms. For example, note the textual and axiomatic definitions of [`water-based planetary surface`](http://purl.obolibrary.org/obo/ENVO_01001193)

Note that if there's no way to satisfactorily axiomatise a class, we don't typically limit the textual definition. In these cases, we would be appreciative if you would add an `editor note` suggesting how this can be done.

## Using ROBOT templates to generate bulk requests
Please see [this page](https://github.com/EnvironmentOntology/envo/wiki/ENVO-Robot-template-and-merge-workflow) for additional guidance on this theme. 


