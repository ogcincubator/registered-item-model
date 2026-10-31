
# Register Item Class (Schema)

`ogc.rim.schemas.register-item-class` *v0.2*

A REGISTER ITEM CLASS model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. (describes the type of object in a register) Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Item Class

## Purpose

A **Register Item Class** is a defined abstraction for register items that share common characteristics. It acts as the type or template against which individual registered items are structured and validated.

The distinction is comparable to a class and its instances: the Register Item Class defines what a kind of item is, while Registered Items are the actual managed entries.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterItemClass`
- class-level object and functional identifiers
- `rim:definedIn`
- `rim:validatedBy`

`rim:realizes` is **not** a property of an item class: its domain is `rim:RegisterItem` and its range
`rim:ConceptVersion`. Whether an item class should relate to a concept of its own is an open
question for the model; until it is settled no such property is offered here.

## Conceptual position

```text
Register
  `-- definedIn (inverse) --> Register Item Class
                                 `-- instantiated by --> Register Items
```

## Dependencies

- `register`
- `concept-version`

These dependencies provide the containing register and the concept-plane terms referenced by this block.

## SHACL validation

`rim:RegisterItemClassShape` (core-ontology `shapes.shacl`): exactly one title; at most one object
and one functional identifier (`xsd:anyURI` literals), at most one defining register, which must be
a `rim:Register`. `validatedBy` is unconstrained in form (an IRI naming the rule set).

## Typical examples

- Dataset class
- Coordinate Reference System class
- API specification class
- Code-list entry class

## Design guidance

Use this block for reusable requirements applying to a category of items. Do not place the values of an individual registered entry here. Those belong in a Registered Item.

## ISO 19135 alignment

This block represents the content-plane abstraction used to define coherent requirements for individual register items.

## Examples

### An item class and the rule set that validates it (JSON)
The "Unit" item class of the register of units. `validatedBy` names the executable rule set
(here a SHACL shape) that expresses the class's data requirements; `definedIn` embeds the
register node so that the base shape's `sh:class rim:Register` check is satisfied by the
example alone.

#### json
```json
{
  "id": "https://example.org/rim/uc",
  "type": "RegisterItemClass",
  "objectIdentifier": "urn:uuid:33333333-3333-4333-8333-333333333333",
  "functionalIdentifier": "https://example.org/classes/unit",
  "title": "Unit",
  "description": "A unit of measure: name, symbol, dimension and conversion factor to the SI coherent unit.",
  "definedIn": { "id": "https://example.org/rim/r", "type": "Register", "title": "Register of Units" },
  "validatedBy": "https://example.org/rules/UnitShape"
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "rim": "https://w3id.org/ogc/rim/",
      "dct": "http://purl.org/dc/terms/",
      "xsd": "http://www.w3.org/2001/XMLSchema#",
      "ex": "https://example.org/rim/"
    },
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item-class/context.jsonld"
  ],
  "id": "https://example.org/rim/uc",
  "type": "RegisterItemClass",
  "objectIdentifier": "urn:uuid:33333333-3333-4333-8333-333333333333",
  "functionalIdentifier": "https://example.org/classes/unit",
  "title": "Unit",
  "description": "A unit of measure: name, symbol, dimension and conversion factor to the SI coherent unit.",
  "definedIn": {
    "id": "https://example.org/rim/r",
    "type": "Register",
    "title": "Register of Units"
  },
  "validatedBy": "https://example.org/rules/UnitShape"
}
```

#### ttl
```ttl
@prefix dct: <http://purl.org/dc/terms/> .
@prefix ex: <https://example.org/rim/> .
@prefix rim: <https://w3id.org/ogc/rim/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:uc a rim:RegisterItemClass ;
    dct:description "A unit of measure: name, symbol, dimension and conversion factor to the SI coherent unit." ;
    dct:title "Unit" ;
    rim:definedIn ex:r ;
    rim:functionalIdentifier "https://example.org/classes/unit"^^xsd:anyURI ;
    rim:objectIdentifier "urn:uuid:33333333-3333-4333-8333-333333333333"^^xsd:anyURI ;
    rim:validatedBy <https://example.org/rules/UnitShape> .

ex:r a rim:Register ;
    dct:title "Register of Units" .


```


### The same item class in Turtle
#### turtle
```turtle
ex:uc a rim:RegisterItemClass ;
    rim:objectIdentifier "urn:uuid:33333333-3333-4333-8333-333333333333"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/classes/unit"^^xsd:anyURI ;
    dct:title "Unit"@en ;
    rim:definedIn ex:r ;
    rim:validatedBy <https://example.org/rules/UnitShape> .

ex:r a rim:Register .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Item Class
description: The abstraction a set of register items share; the unit data requirements
  and validation rules attach to (ISO 19135:2026). `type` is not pinned so that a
  sub-register may use its own term.
type: object
required:
- id
- type
- title
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    oneOf:
    - type: string
    - type: array
      minItems: 1
      items:
        type: string
    x-jsonld-id: '@type'
  objectIdentifier:
    type: string
    format: uri
    x-jsonld-id: https://w3id.org/ogc/rim/objectIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  functionalIdentifier:
    type: string
    format: uri
    x-jsonld-id: https://w3id.org/ogc/rim/functionalIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  title:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/title
  description:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/description
  definedIn:
    description: The register that defines this item class (an IRI or an embedded
      rim:Register node).
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/definedIn
    x-jsonld-type: '@id'
  validatedBy:
    description: The executable rule set (e.g. a SHACL shape) expressing this class's
      data requirements.
    oneOf:
    - type: string
      format: uri
    - type: array
      items:
        type: string
        format: uri
    x-jsonld-id: https://w3id.org/ogc/rim/validatedBy
    x-jsonld-type: '@id'
additionalProperties: true
$defs:
  ref:
    oneOf:
    - type: string
      format: uri
    - type: object
      required:
      - id
      properties:
        id:
          type: string
          format: uri
          x-jsonld-id: '@id'
x-jsonld-extra-terms:
  Register: https://w3id.org/ogc/rim/Register
  RegisterSystem: https://w3id.org/ogc/rim/RegisterSystem
  RegisterSpecification: https://w3id.org/ogc/rim/RegisterSpecification
  RegisterItemClass: https://w3id.org/ogc/rim/RegisterItemClass
  RegisterItem: https://w3id.org/ogc/rim/RegisterItem
  RegisterAction: https://w3id.org/ogc/rim/RegisterAction
  Concept: https://w3id.org/ogc/rim/Concept
  ConceptVersion: https://w3id.org/ogc/rim/ConceptVersion
  ConceptSystem: https://w3id.org/ogc/rim/ConceptSystem
  Commitment: https://w3id.org/ogc/rim/Commitment
  Agent: http://www.w3.org/ns/prov#Agent
  Organization: http://www.w3.org/ns/prov#Organization
  Person: http://www.w3.org/ns/prov#Person
  label: http://www.w3.org/2000/01/rdf-schema#label
  prefLabel: http://www.w3.org/2004/02/skos/core#prefLabel
  definition: http://www.w3.org/2004/02/skos/core#definition
  inScheme:
    x-jsonld-id: http://www.w3.org/2004/02/skos/core#inScheme
    x-jsonld-type: '@id'
  runsOn:
    x-jsonld-id: https://w3id.org/ogc/rim/runsOn
    x-jsonld-type: '@id'
  hasSpecification:
    x-jsonld-id: https://w3id.org/ogc/rim/hasSpecification
    x-jsonld-type: '@id'
  definesSubstantiveChange: https://w3id.org/ogc/rim/definesSubstantiveChange
  registerOwner:
    x-jsonld-id: https://w3id.org/ogc/rim/registerOwner
    x-jsonld-type: '@id'
  registerManager:
    x-jsonld-id: https://w3id.org/ogc/rim/registerManager
    x-jsonld-type: '@id'
  controlBody:
    x-jsonld-id: https://w3id.org/ogc/rim/controlBody
    x-jsonld-type: '@id'
  registerSystemManager:
    x-jsonld-id: https://w3id.org/ogc/rim/registerSystemManager
    x-jsonld-type: '@id'
  registerUser:
    x-jsonld-id: https://w3id.org/ogc/rim/registerUser
    x-jsonld-type: '@id'
  hasCommitment:
    x-jsonld-id: https://w3id.org/ogc/rim/hasCommitment
    x-jsonld-type: '@id'
  commitmentCategory:
    x-jsonld-id: https://w3id.org/ogc/rim/commitmentCategory
    x-jsonld-type: '@vocab'
  access: https://w3id.org/ogc/rim/accessCommitment
  persistence: https://w3id.org/ogc/rim/persistenceCommitment
  transparency: https://w3id.org/ogc/rim/transparencyCommitment
  itemClass:
    x-jsonld-id: https://w3id.org/ogc/rim/itemClass
    x-jsonld-type: '@id'
  inRegister:
    x-jsonld-id: https://w3id.org/ogc/rim/inRegister
    x-jsonld-type: '@id'
  realizes:
    x-jsonld-id: https://w3id.org/ogc/rim/realizes
    x-jsonld-type: '@id'
  versionOf:
    x-jsonld-id: https://w3id.org/ogc/rim/versionOf
    x-jsonld-type: '@id'
  version: https://w3id.org/ogc/rim/version
  validityStatus:
    x-jsonld-id: https://w3id.org/ogc/rim/validityStatus
    x-jsonld-type: '@vocab'
  publicationStatus:
    x-jsonld-id: https://w3id.org/ogc/rim/publicationStatus
    x-jsonld-type: '@vocab'
  valid: https://w3id.org/ogc/rim/valid
  invalid: https://w3id.org/ogc/rim/invalid
  published: https://w3id.org/ogc/rim/published
  unpublished: https://w3id.org/ogc/rim/unpublished
  isRedacted:
    x-jsonld-id: https://w3id.org/ogc/rim/isRedacted
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  isDeleted:
    x-jsonld-id: https://w3id.org/ogc/rim/isDeleted
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  relatedItem:
    x-jsonld-id: https://w3id.org/ogc/rim/relatedItem
    x-jsonld-type: '@id'
  supersededBy:
    x-jsonld-id: https://w3id.org/ogc/rim/supersededBy
    x-jsonld-type: '@id'
  supersedes:
    x-jsonld-id: https://w3id.org/ogc/rim/supersedes
    x-jsonld-type: '@id'
  hasAction:
    x-jsonld-id: https://w3id.org/ogc/rim/hasAction
    x-jsonld-type: '@id'
  appliesTo:
    x-jsonld-id: https://w3id.org/ogc/rim/appliesTo
    x-jsonld-type: '@id'
  proposer:
    x-jsonld-id: https://w3id.org/ogc/rim/proposer
    x-jsonld-type: '@id'
  actionType:
    x-jsonld-id: https://w3id.org/ogc/rim/actionType
    x-jsonld-type: '@vocab'
  addition: https://w3id.org/ogc/rim/additionAction
  publish: https://w3id.org/ogc/rim/publishAction
  unpublish: https://w3id.org/ogc/rim/unpublishAction
  invalidation: https://w3id.org/ogc/rim/invalidationAction
  supersession: https://w3id.org/ogc/rim/supersessionAction
  redaction: https://w3id.org/ogc/rim/redactionAction
  deletion: https://w3id.org/ogc/rim/deletionAction
  changeClassification:
    x-jsonld-id: https://w3id.org/ogc/rim/changeClassification
    x-jsonld-type: '@vocab'
  substantive: https://w3id.org/ogc/rim/substantiveChange
  clarifying: https://w3id.org/ogc/rim/clarifyingChange
  correctional: https://w3id.org/ogc/rim/correctionalChange
  startedAtTime:
    x-jsonld-id: http://www.w3.org/ns/prov#startedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  endedAtTime:
    x-jsonld-id: http://www.w3.org/ns/prov#endedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  wasAssociatedWith:
    x-jsonld-id: http://www.w3.org/ns/prov#wasAssociatedWith
    x-jsonld-type: '@id'
x-jsonld-prefixes:
  rim: https://w3id.org/ogc/rim/
  prov: http://www.w3.org/ns/prov#
  dct: http://purl.org/dc/terms/
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  skos: http://www.w3.org/2004/02/skos/core#
  xsd: http://www.w3.org/2001/XMLSchema#
x-bblocks-metadata:
  itemIdentifier: ogc.rim.schemas.register-item-class
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item-class/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item-class/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "Register": "rim:Register",
    "RegisterSystem": "rim:RegisterSystem",
    "RegisterSpecification": "rim:RegisterSpecification",
    "RegisterItemClass": "rim:RegisterItemClass",
    "RegisterItem": "rim:RegisterItem",
    "RegisterAction": "rim:RegisterAction",
    "Concept": "rim:Concept",
    "ConceptVersion": "rim:ConceptVersion",
    "ConceptSystem": "rim:ConceptSystem",
    "Commitment": "rim:Commitment",
    "Agent": "prov:Agent",
    "Organization": "prov:Organization",
    "Person": "prov:Person",
    "label": "rdfs:label",
    "prefLabel": "skos:prefLabel",
    "definition": "skos:definition",
    "inScheme": {
      "@id": "skos:inScheme",
      "@type": "@id"
    },
    "runsOn": {
      "@id": "rim:runsOn",
      "@type": "@id"
    },
    "hasSpecification": {
      "@id": "rim:hasSpecification",
      "@type": "@id"
    },
    "definesSubstantiveChange": "rim:definesSubstantiveChange",
    "registerOwner": {
      "@id": "rim:registerOwner",
      "@type": "@id"
    },
    "registerManager": {
      "@id": "rim:registerManager",
      "@type": "@id"
    },
    "controlBody": {
      "@id": "rim:controlBody",
      "@type": "@id"
    },
    "registerSystemManager": {
      "@id": "rim:registerSystemManager",
      "@type": "@id"
    },
    "registerUser": {
      "@id": "rim:registerUser",
      "@type": "@id"
    },
    "hasCommitment": {
      "@id": "rim:hasCommitment",
      "@type": "@id"
    },
    "commitmentCategory": {
      "@id": "rim:commitmentCategory",
      "@type": "@vocab"
    },
    "access": "rim:accessCommitment",
    "persistence": "rim:persistenceCommitment",
    "transparency": "rim:transparencyCommitment",
    "itemClass": {
      "@id": "rim:itemClass",
      "@type": "@id"
    },
    "inRegister": {
      "@id": "rim:inRegister",
      "@type": "@id"
    },
    "realizes": {
      "@id": "rim:realizes",
      "@type": "@id"
    },
    "versionOf": {
      "@id": "rim:versionOf",
      "@type": "@id"
    },
    "version": "rim:version",
    "validityStatus": {
      "@id": "rim:validityStatus",
      "@type": "@vocab"
    },
    "publicationStatus": {
      "@id": "rim:publicationStatus",
      "@type": "@vocab"
    },
    "valid": "rim:valid",
    "invalid": "rim:invalid",
    "published": "rim:published",
    "unpublished": "rim:unpublished",
    "isRedacted": {
      "@id": "rim:isRedacted",
      "@type": "xsd:boolean"
    },
    "isDeleted": {
      "@id": "rim:isDeleted",
      "@type": "xsd:boolean"
    },
    "relatedItem": {
      "@id": "rim:relatedItem",
      "@type": "@id"
    },
    "supersededBy": {
      "@id": "rim:supersededBy",
      "@type": "@id"
    },
    "supersedes": {
      "@id": "rim:supersedes",
      "@type": "@id"
    },
    "hasAction": {
      "@id": "rim:hasAction",
      "@type": "@id"
    },
    "appliesTo": {
      "@id": "rim:appliesTo",
      "@type": "@id"
    },
    "proposer": {
      "@id": "rim:proposer",
      "@type": "@id"
    },
    "actionType": {
      "@id": "rim:actionType",
      "@type": "@vocab"
    },
    "addition": "rim:additionAction",
    "publish": "rim:publishAction",
    "unpublish": "rim:unpublishAction",
    "invalidation": "rim:invalidationAction",
    "supersession": "rim:supersessionAction",
    "redaction": "rim:redactionAction",
    "deletion": "rim:deletionAction",
    "changeClassification": {
      "@id": "rim:changeClassification",
      "@type": "@vocab"
    },
    "substantive": "rim:substantiveChange",
    "clarifying": "rim:clarifyingChange",
    "correctional": "rim:correctionalChange",
    "startedAtTime": {
      "@id": "prov:startedAtTime",
      "@type": "xsd:dateTime"
    },
    "endedAtTime": {
      "@id": "prov:endedAtTime",
      "@type": "xsd:dateTime"
    },
    "wasAssociatedWith": {
      "@id": "prov:wasAssociatedWith",
      "@type": "@id"
    },
    "id": "@id",
    "type": "@type",
    "objectIdentifier": {
      "@id": "rim:objectIdentifier",
      "@type": "xsd:anyURI"
    },
    "functionalIdentifier": {
      "@id": "rim:functionalIdentifier",
      "@type": "xsd:anyURI"
    },
    "title": "dct:title",
    "description": "dct:description",
    "definedIn": {
      "@id": "rim:definedIn",
      "@type": "@id"
    },
    "validatedBy": {
      "@id": "rim:validatedBy",
      "@type": "@id"
    },
    "rim": "https://w3id.org/ogc/rim/",
    "prov": "http://www.w3.org/ns/prov#",
    "dct": "http://purl.org/dc/terms/",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item-class/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/register-item-class`

