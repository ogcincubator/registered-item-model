
# Register Item (Schema)

`ogc.rim.schemas.register-item` *v0.2*

A REGISTER ITEM model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. This provides a canonical schema for a register entry, that can be extended according the REGISTER ITEM CLASS defined for the register.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Registered Item

## Purpose

A **Registered Item** is an actual managed unit of information in a register. It is an instance of a Register Item Class and can realize a Concept Version from the concept plane.

This block represents operational register content rather than the template that constrains that content.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterItem`
- item-level object and functional identifiers
- `rim:itemClass`
- `rim:inRegister`
- `rim:realizes`
- `rim:validityStatus`, `rim:publicationStatus`, `rim:isRedacted`, `rim:isDeleted`
- `rim:relatedItem`
- `rim:supersededBy`
- `rim:supersedes`
- `rim:hasAction`

`schema.yaml` requires exactly the ISO 19135:2026 required core -- `objectIdentifier`,
`itemClass`, `validityStatus`, `publicationStatus` -- and nothing else, so that a sub-register can
`allOf`-reference it and add its own content properties. `type` is deliberately not pinned to
`RegisterItem`: a sub-register uses its own type term, declared in its ontology as a sub-class of
`rim:RegisterItem`, and the base shape then applies to its items through the ontology closure.

Status values are the model's own SKOS concepts: `valid` / `invalid` map to `rim:valid` /
`rim:invalid`, `published` / `unpublished` to `rim:published` / `rim:unpublished`. A register must
not mint its own.

## Conceptual position

```text
Register Item Class
  `-- instance --> Registered Item
                       |-- inRegister --> Register
                       |-- realizes --> Concept Version
                       `-- relates to / supersedes --> Registered Item
```

## Dependencies

- `register`
- `register-item-class`
- `concept-version`

## SHACL validation

`rim:RegisterItemShape` (core-ontology `shapes.shacl`): exactly one object identifier
(`xsd:anyURI` literal), exactly one item class that is a `rim:RegisterItemClass`, exactly one
validity and one publication status drawn from the model's values; at most one functional
identifier, one register (a `rim:Register`) and one realised concept version. Related, superseding
and superseded items must themselves be `rim:RegisterItem`s.

`tests/item-class-untyped-fail.json` is the negative case: it passes the JSON Schema and is
rejected because its item class is not a declared `rim:RegisterItemClass`.

## Typical examples

- the registered entry for EPSG:4326
- an AHN dataset entry
- a registered release of DCAT-AP-NL
- an individual code-list value

## Register Item Class versus Registered Item

```text
Register Item Class: Coordinate Reference System
Registered Item:      EPSG:4326
```

The class supplies common requirements. The item supplies the actual governed content.

## ISO 19135 alignment

This block represents the concrete content-plane unit. Register actions that modify its state or record its history are defined separately in the Register Action block.

## Examples

### A register item realising a concept version (JSON)
The registered "metre", with the ISO 19135:2026 required core (object identifier, item class,
validity and publication status) plus its register and the concept version it realises. The
referenced nodes are embedded with their `type` so that the base shape's `sh:class` checks
hold on this document alone; in a real register they would be separate resources referenced
by IRI.

#### json
```json
{
  "id": "https://example.org/rim/i",
  "type": "RegisterItem",
  "objectIdentifier": "urn:uuid:44444444-4444-4444-8444-444444444444",
  "functionalIdentifier": "https://example.org/items/metre",
  "title": "metre",
  "itemClass": { "id": "https://example.org/rim/uc", "type": "RegisterItemClass", "title": "Unit" },
  "inRegister": { "id": "https://example.org/rim/r", "type": "Register", "title": "Register of Units" },
  "realizes": {
    "id": "https://example.org/rim/cv",
    "type": "ConceptVersion",
    "versionOf": { "id": "https://example.org/rim/c", "type": "Concept", "prefLabel": "metre" },
    "version": "1.0",
    "objectIdentifier": "urn:uuid:22222222-2222-4222-8222-222222222222",
    "validityStatus": "valid",
    "publicationStatus": "published"
  },
  "validityStatus": "valid",
  "publicationStatus": "published"
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "rim": "https://w3id.org/ogc/rim/",
      "dct": "http://purl.org/dc/terms/",
      "skos": "http://www.w3.org/2004/02/skos/core#",
      "xsd": "http://www.w3.org/2001/XMLSchema#",
      "ex": "https://example.org/rim/"
    },
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item/context.jsonld"
  ],
  "id": "https://example.org/rim/i",
  "type": "RegisterItem",
  "objectIdentifier": "urn:uuid:44444444-4444-4444-8444-444444444444",
  "functionalIdentifier": "https://example.org/items/metre",
  "title": "metre",
  "itemClass": {
    "id": "https://example.org/rim/uc",
    "type": "RegisterItemClass",
    "title": "Unit"
  },
  "inRegister": {
    "id": "https://example.org/rim/r",
    "type": "Register",
    "title": "Register of Units"
  },
  "realizes": {
    "id": "https://example.org/rim/cv",
    "type": "ConceptVersion",
    "versionOf": {
      "id": "https://example.org/rim/c",
      "type": "Concept",
      "prefLabel": "metre"
    },
    "version": "1.0",
    "objectIdentifier": "urn:uuid:22222222-2222-4222-8222-222222222222",
    "validityStatus": "valid",
    "publicationStatus": "published"
  },
  "validityStatus": "valid",
  "publicationStatus": "published"
}
```

#### ttl
```ttl
@prefix dct: <http://purl.org/dc/terms/> .
@prefix ex: <https://example.org/rim/> .
@prefix rim: <https://w3id.org/ogc/rim/> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:i a rim:RegisterItem ;
    dct:title "metre" ;
    rim:functionalIdentifier "https://example.org/items/metre"^^xsd:anyURI ;
    rim:inRegister ex:r ;
    rim:itemClass ex:uc ;
    rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI ;
    rim:publicationStatus rim:published ;
    rim:realizes ex:cv ;
    rim:validityStatus rim:valid .

ex:c a rim:Concept ;
    skos:prefLabel "metre" .

ex:cv a rim:ConceptVersion ;
    rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI ;
    rim:publicationStatus rim:published ;
    rim:validityStatus rim:valid ;
    rim:version "1.0" ;
    rim:versionOf ex:c .

ex:r a rim:Register ;
    dct:title "Register of Units" .

ex:uc a rim:RegisterItemClass ;
    dct:title "Unit" .


```


### The same item in Turtle
#### turtle
```turtle
ex:i a rim:RegisterItem ;
    rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/items/metre"^^xsd:anyURI ;
    rim:itemClass ex:uc ;
    rim:inRegister ex:r ;
    rim:realizes ex:cv ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    dct:title "metre"@en .

ex:uc a rim:RegisterItemClass ;
    dct:title "Unit"@en ;
    rim:definedIn ex:r .

ex:r a rim:Register ;
    dct:title "Register of Units"@en .

ex:cv a rim:ConceptVersion ;
    rim:versionOf ex:c ;
    rim:version "1.0" ;
    rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published .

ex:c a rim:Concept ;
    skos:prefLabel "metre"@en .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Item
description: 'A register item: the concrete record in a register''s content plane
  (ISO 19135:2026). Pins the model''s required core -- object identifier, item class,
  validity and publication status -- and nothing else, so that a sub-register can
  `allOf`-reference this schema and add its own content properties. `type` is not
  pinned: a sub-register uses its own type term, declared in its ontology as a sub-class
  of rim:RegisterItem, and the base SHACL shape then applies through that sub-class.'
type: object
required:
- id
- type
- objectIdentifier
- itemClass
- validityStatus
- publicationStatus
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    description: rim:RegisterItem, or a sub-register's own term for a sub-class of
      it.
    oneOf:
    - type: string
    - type: array
      minItems: 1
      items:
        type: string
    x-jsonld-id: '@type'
  objectIdentifier:
    description: Permanently bound to this item; never redirects.
    type: string
    format: uri
    x-jsonld-id: https://w3id.org/ogc/rim/objectIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  functionalIdentifier:
    description: Redirectable identifier serving a stable intent (e.g. "the current
      version").
    type: string
    format: uri
    x-jsonld-id: https://w3id.org/ogc/rim/functionalIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  itemClass:
    description: The item's one rim:RegisterItemClass (an IRI or an embedded node).
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/itemClass
    x-jsonld-type: '@id'
  inRegister:
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/inRegister
    x-jsonld-type: '@id'
  realizes:
    description: The rim:ConceptVersion this item represents, where the register has
      a concept plane.
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/realizes
    x-jsonld-type: '@id'
  validityStatus:
    enum:
    - valid
    - invalid
    x-jsonld-id: https://w3id.org/ogc/rim/validityStatus
    x-jsonld-type: '@vocab'
  publicationStatus:
    enum:
    - published
    - unpublished
    x-jsonld-id: https://w3id.org/ogc/rim/publicationStatus
    x-jsonld-type: '@vocab'
  isRedacted:
    type: boolean
    x-jsonld-id: https://w3id.org/ogc/rim/isRedacted
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  isDeleted:
    type: boolean
    x-jsonld-id: https://w3id.org/ogc/rim/isDeleted
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#boolean
  title:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/title
  description:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/description
  relatedItem:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/relatedItem
    x-jsonld-type: '@id'
  supersededBy:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/supersededBy
    x-jsonld-type: '@id'
  supersedes:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/supersedes
    x-jsonld-type: '@id'
  hasAction:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/hasAction
    x-jsonld-type: '@id'
additionalProperties: true
$defs:
  ref:
    description: An IRI, or an embedded node carrying at least its `id`.
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
  refs:
    oneOf:
    - $ref: '#/$defs/ref'
    - type: array
      items:
        $ref: '#/$defs/ref'
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
  definedIn:
    x-jsonld-id: https://w3id.org/ogc/rim/definedIn
    x-jsonld-type: '@id'
  validatedBy:
    x-jsonld-id: https://w3id.org/ogc/rim/validatedBy
    x-jsonld-type: '@id'
  versionOf:
    x-jsonld-id: https://w3id.org/ogc/rim/versionOf
    x-jsonld-type: '@id'
  version: https://w3id.org/ogc/rim/version
  valid: https://w3id.org/ogc/rim/valid
  invalid: https://w3id.org/ogc/rim/invalid
  published: https://w3id.org/ogc/rim/published
  unpublished: https://w3id.org/ogc/rim/unpublished
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
  itemIdentifier: ogc.rim.schemas.register-item
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item/schema.yaml)


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
    "definedIn": {
      "@id": "rim:definedIn",
      "@type": "@id"
    },
    "validatedBy": {
      "@id": "rim:validatedBy",
      "@type": "@id"
    },
    "versionOf": {
      "@id": "rim:versionOf",
      "@type": "@id"
    },
    "version": "rim:version",
    "valid": "rim:valid",
    "invalid": "rim:invalid",
    "published": "rim:published",
    "unpublished": "rim:unpublished",
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
    "validityStatus": {
      "@id": "rim:validityStatus",
      "@type": "@vocab"
    },
    "publicationStatus": {
      "@id": "rim:publicationStatus",
      "@type": "@vocab"
    },
    "isRedacted": {
      "@id": "rim:isRedacted",
      "@type": "xsd:boolean"
    },
    "isDeleted": {
      "@id": "rim:isDeleted",
      "@type": "xsd:boolean"
    },
    "title": "dct:title",
    "description": "dct:description",
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
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-item/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/register-item`

