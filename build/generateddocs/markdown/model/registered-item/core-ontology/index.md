
# Registered Item Model (Model)

`ogc.model.registered-item.core-ontology` *v0.1*

A base RDF and SHACL model for register items, register item classes and register governance metadata, based on ISO 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### A governed concept register
A national vocabulary register for plant taxa: it maintains both a concept plane
(the taxon as a unit of meaning) and a content plane (the register item that
represents it), assigns all six governance roles, and records the action and
change classification behind a status change. This is the pattern a "governed
concept register" (the second-most capable of the five ISO 19135:2026 register
types) maps onto.

#### turtle
```turtle
ex:plantRegister a rim:Register ;
    dct:title "National Plant Taxa Register" ;
    rim:runsOn ex:plantRegisterSystem ;
    rim:hasSpecification ex:plantRegisterSpec ;
    rim:registerOwner ex:nationalHerbarium ;
    rim:registerManager ex:vocabServiceTeam ;
    rim:controlBody ex:taxonomyEditorialBoard ;
    rim:registerSystemManager ex:itOperations ;
    rim:hasCommitment ex:persistenceCommitmentA .

ex:plantRegisterSystem a rim:RegisterSystem .

ex:plantRegisterSpec a rim:RegisterSpecification ;
    dct:title "National Plant Taxa Register Specification" ;
    rim:definesSubstantiveChange "A change is substantive if it alters a taxon's accepted name or its placement in the classification hierarchy." .

ex:persistenceCommitmentA a rim:Commitment ;
    rim:commitmentCategory rim:persistenceCommitment ;
    dct:description "Object identifiers, once assigned, are never reused or reassigned." .

ex:nationalHerbarium a prov:Agent ; dct:title "National Herbarium" .
ex:vocabServiceTeam a prov:Agent ; dct:title "Vocabulary Service Team" .
ex:taxonomyEditorialBoard a prov:Agent ; dct:title "Taxonomy Editorial Board" .
ex:itOperations a prov:Agent ; dct:title "IT Operations" .

ex:taxonSystem a rim:ConceptSystem ;
    skos:prefLabel "Plant taxa"@en .

ex:quercusRoburConcept a rim:Concept ;
    skos:inScheme ex:taxonSystem ;
    skos:prefLabel "Quercus robur"@en ;
    skos:definition "The pedunculate oak, a species of oak in the family Fagaceae."@en .

ex:quercusRoburV2 a rim:ConceptVersion ;
    rim:versionOf ex:quercusRoburConcept ;
    rim:objectIdentifier "https://example.org/registers/plants/concepts/quercus-robur/v2"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/registers/plants/concepts/quercus-robur/current"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published .

ex:taxonItemClass a rim:RegisterItemClass ;
    dct:title "Taxon record" ;
    dct:description "The register item class for a single accepted plant taxon." ;
    rim:definedIn ex:plantRegister ;
    rim:validatedBy <https://example.org/registers/plants/shapes#TaxonRecordShape> .

ex:quercusRoburItem a rim:RegisterItem ;
    rim:inRegister ex:plantRegister ;
    rim:itemClass ex:taxonItemClass ;
    rim:objectIdentifier "https://example.org/registers/plants/items/quercus-robur/v2"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/registers/plants/items/quercus-robur/current"^^xsd:anyURI ;
    dct:title "Quercus robur"@en ;
    rim:realizes ex:quercusRoburV2 ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    rim:hasAction ex:action2024-06 .

ex:action2024-06 a rim:RegisterAction ;
    rim:actionType rim:publishAction ;
    rim:changeClassification rim:clarifyingChange ;
    rim:appliesTo ex:quercusRoburItem ;
    rim:proposer ex:vocabServiceTeam ;
    prov:startedAtTime "2024-06-01T00:00:00Z"^^xsd:dateTime .

```


### A minimal content register item
The lightest of the five ISO 19135:2026 conformant register types — a "content
register" — has no concept plane and no governance requirements class. A register
item still needs only what the base block requires: an item class, an object
identifier, and validity and publication status.

#### turtle
```turtle
ex:codeListItemClass a rim:RegisterItemClass ;
    dct:title "Code list entry" .

ex:code42 a rim:RegisterItem ;
    rim:itemClass ex:codeListItemClass ;
    rim:objectIdentifier "https://example.org/registers/codes/42"^^xsd:anyURI ;
    dct:title "Example code 42"@en ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published .

```

## Sources

* [ISO 19135:2026, Geographic information — Registration and register governance](https://www.iso.org/standard/86261.html)
* [Registers and register governance for federated semantic resources (companion analysis)](https://github.com/ogcincubator/bblocks-docs/blob/master/design/register-governance.md)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/core-ontology`

