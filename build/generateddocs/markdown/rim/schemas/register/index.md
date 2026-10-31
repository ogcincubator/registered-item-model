
# Register (Schema)

`ogc.rim.schemas.register` *v0.2*

A REGISTER description model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register

## Purpose

A **Register** is the managed collection at the centre of the ISO 19135 registration framework. It provides the organizational boundary within which information is identified, maintained and made accessible through governed processes.

This block represents the register itself and the information system on which it is maintained. It deliberately does not define the governance document, registered content, semantic concepts or change actions. Those concerns are provided by separate blocks.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:Register`
- `rim:RegisterSystem`
- register-level object and functional identifiers
- `rim:runsOn`, linking a register to its supporting system
- the six governance roles (`rim:registerOwner`, `rim:registerManager`, `rim:controlBody`,
  `rim:registerSystemManager`, `rim:registerUser`; `rim:proposer` is on actions)

`schema.yaml` lists these as JSON properties; `context.jsonld` maps them onto the ontology. `type`
is deliberately not pinned to `Register`, so that a sub-register can `allOf`-reference this schema
while using its own type term (a sub-class of `rim:Register` in its own ontology).

## Conceptual position

```text
Register
  |-- runsOn --> Register System
  |-- hasSpecification --> Register Specification
  `-- contains governed Register Items
```

The last two relationships are defined by dependent blocks, keeping this base block independent.

## Dependencies

`core-ontology`, which supplies every term and the SHACL shapes.

## SHACL validation

`rim:RegisterShape` (core-ontology `shapes.shacl`): at most one title, at most one object and one
functional identifier (each an `xsd:anyURI` literal), at most one specification and one register
system; commitments, if any, must be `rim:Commitment`s. Nothing is required: the ISO 19135:2026
required core applies to items and concept versions, not to the register node itself.

## Typical uses

- coordinate reference system register
- vocabulary or code-list register
- metadata profile register
- API or specification register

## ISO 19135 alignment

The block implements a technology-specific RDF representation of the ISO 19135 register concept. It is an implementation profile, not a claim that SHACL validation alone establishes full ISO 19135 conformance.

## Examples

### A register, its system and its roles (JSON)
The register of units, with its two identifiers, the system it runs on and three of the six
ISO 19135:2026 roles. The JSON-LD context maps every key onto `rim:` / `dct:` / `prov:`.

#### json
```json
{
  "id": "https://example.org/rim/r",
  "type": "Register",
  "objectIdentifier": "urn:uuid:11111111-1111-4111-8111-111111111111",
  "functionalIdentifier": "https://example.org/registers/units",
  "title": "Register of Units",
  "description": "Units of measure in use across the organisation's datasets.",
  "runsOn": { "id": "https://example.org/rim/sys", "type": "RegisterSystem", "label": "Units register system" },
  "registerOwner": { "id": "https://example.org/org/metrology", "type": "Organization", "label": "Metrology office" },
  "registerManager": "https://example.org/org/metrology",
  "controlBody": "https://example.org/org/units-panel"
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "rim": "https://w3id.org/ogc/rim/",
      "dct": "http://purl.org/dc/terms/",
      "prov": "http://www.w3.org/ns/prov#",
      "xsd": "http://www.w3.org/2001/XMLSchema#",
      "ex": "https://example.org/rim/"
    },
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register/context.jsonld"
  ],
  "id": "https://example.org/rim/r",
  "type": "Register",
  "objectIdentifier": "urn:uuid:11111111-1111-4111-8111-111111111111",
  "functionalIdentifier": "https://example.org/registers/units",
  "title": "Register of Units",
  "description": "Units of measure in use across the organisation's datasets.",
  "runsOn": {
    "id": "https://example.org/rim/sys",
    "type": "RegisterSystem",
    "label": "Units register system"
  },
  "registerOwner": {
    "id": "https://example.org/org/metrology",
    "type": "Organization",
    "label": "Metrology office"
  },
  "registerManager": "https://example.org/org/metrology",
  "controlBody": "https://example.org/org/units-panel"
}
```

#### ttl
```ttl
@prefix dct: <http://purl.org/dc/terms/> .
@prefix ex: <https://example.org/rim/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rim: <https://w3id.org/ogc/rim/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:r a rim:Register ;
    dct:description "Units of measure in use across the organisation's datasets." ;
    dct:title "Register of Units" ;
    rim:controlBody <https://example.org/org/units-panel> ;
    rim:functionalIdentifier "https://example.org/registers/units"^^xsd:anyURI ;
    rim:objectIdentifier "urn:uuid:11111111-1111-4111-8111-111111111111"^^xsd:anyURI ;
    rim:registerManager <https://example.org/org/metrology> ;
    rim:registerOwner <https://example.org/org/metrology> ;
    rim:runsOn ex:sys .

ex:sys a rim:RegisterSystem ;
    rdfs:label "Units register system" .

<https://example.org/org/metrology> a prov:Organization ;
    rdfs:label "Metrology office" .


```


### The same register in Turtle
#### turtle
```turtle
ex:r a rim:Register ;
    rim:objectIdentifier "urn:uuid:11111111-1111-4111-8111-111111111111"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/registers/units"^^xsd:anyURI ;
    dct:title "Register of Units"@en ;
    rim:runsOn ex:sys ;
    rim:registerOwner <https://example.org/org/metrology> .

ex:sys a rim:RegisterSystem .
<https://example.org/org/metrology> a prov:Organization .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register
description: 'A register (ISO 19135:2026): the governed collection itself, distinct
  from the register system it runs on and from the register specification that documents
  its governance. `type` is not pinned so that a sub-register may use its own type
  term (a sub-class of rim:Register in its ontology).'
type: object
required:
- id
- type
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    description: rim:Register, or a sub-register's own term for a sub-class of it.
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
  runsOn:
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/runsOn
    x-jsonld-type: '@id'
  hasSpecification:
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/hasSpecification
    x-jsonld-type: '@id'
  registerOwner:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/registerOwner
    x-jsonld-type: '@id'
  registerManager:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/registerManager
    x-jsonld-type: '@id'
  controlBody:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/controlBody
    x-jsonld-type: '@id'
  registerSystemManager:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/registerSystemManager
    x-jsonld-type: '@id'
  registerUser:
    $ref: '#/$defs/refs'
    x-jsonld-id: https://w3id.org/ogc/rim/registerUser
    x-jsonld-type: '@id'
  hasCommitment:
    type: array
    items:
      type: object
      required:
      - commitmentCategory
      - description
      properties:
        id:
          type: string
          format: uri
          x-jsonld-id: '@id'
        type:
          const: Commitment
          x-jsonld-id: '@type'
        commitmentCategory:
          enum:
          - access
          - persistence
          - transparency
          x-jsonld-id: https://w3id.org/ogc/rim/commitmentCategory
          x-jsonld-type: '@vocab'
        description:
          type: string
          x-jsonld-id: http://purl.org/dc/terms/description
    x-jsonld-id: https://w3id.org/ogc/rim/hasCommitment
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
  definesSubstantiveChange: https://w3id.org/ogc/rim/definesSubstantiveChange
  access: https://w3id.org/ogc/rim/accessCommitment
  persistence: https://w3id.org/ogc/rim/persistenceCommitment
  transparency: https://w3id.org/ogc/rim/transparencyCommitment
  definedIn:
    x-jsonld-id: https://w3id.org/ogc/rim/definedIn
    x-jsonld-type: '@id'
  validatedBy:
    x-jsonld-id: https://w3id.org/ogc/rim/validatedBy
    x-jsonld-type: '@id'
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
  itemIdentifier: ogc.rim.schemas.register
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register/schema.yaml)


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
    "definesSubstantiveChange": "rim:definesSubstantiveChange",
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
    "runsOn": {
      "@id": "rim:runsOn",
      "@type": "@id"
    },
    "hasSpecification": {
      "@id": "rim:hasSpecification",
      "@type": "@id"
    },
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
      "@context": {
        "commitmentCategory": {
          "@id": "rim:commitmentCategory",
          "@type": "@vocab"
        }
      },
      "@id": "rim:hasCommitment",
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
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/register`

