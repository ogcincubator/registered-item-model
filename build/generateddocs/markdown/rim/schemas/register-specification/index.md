
# Register Specification (Schema)

`ogc.rim.schemas.register-specification` *v0.2*

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Specification

## Purpose

A **Register Specification** documents the governance and requirements of a register and its contents. It explains what the register manages, how identifiers and versions are assigned, which actions and relations are allowed, and which commitments the register makes to its users.

Separating this block from the Register block keeps the managed container distinct from the governance rules that control it.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterSpecification`
- `rim:Commitment` and `rim:commitmentCategory` (values: `rim:accessCommitment`,
  `rim:persistenceCommitment`, `rim:transparencyCommitment`)
- `rim:hasSpecification`
- `rim:hasCommitment`
- `rim:definesSubstantiveChange`

## Conceptual position

```text
Register
  `-- hasSpecification --> Register Specification
                                |-- hasCommitment --> Commitment
                                `-- defines governance and change rules
```

## Dependencies

- `register`

The dependency provides `rim:Register`, which is the domain of `rim:hasSpecification`.

## SHACL validation

`rim:RegisterSpecificationShape` and `rim:CommitmentShape` (core-ontology `shapes.shacl`): a
specification must have exactly one title and may have descriptions and one definition of
substantive change; each commitment requires one category from the model's scheme and a
description.

## Typical content

A full specification can document:

- purpose and scope of the register
- intended users and accessibility needs
- roles and responsibilities
- identifier and versioning schemes
- allowed statuses, relations and actions
- content requirements and validation rules
- persistence, traceability and access commitments

## ISO 19135 alignment

This block represents the documented governance layer. Most governance rules cannot be verified from a single RDF resource, so operational conformance tests remain necessary in addition to these structural SHACL constraints.

## Examples

### A register specification with two commitments (JSON)
#### json
```json
{
  "id": "https://example.org/rim/s",
  "type": "RegisterSpecification",
  "title": "Units register specification",
  "definesSubstantiveChange": "Any change to a unit's symbol, dimension or conversion factor is substantive; changes to its description are clarifying.",
  "hasCommitment": [
    { "type": "Commitment", "commitmentCategory": "persistence", "description": "Object identifiers are never reassigned or deleted." },
    { "type": "Commitment", "commitmentCategory": "access", "description": "Current and historic content are readable without authentication." }
  ]
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "rim": "https://w3id.org/ogc/rim/",
      "dct": "http://purl.org/dc/terms/",
      "ex": "https://example.org/rim/"
    },
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-specification/context.jsonld"
  ],
  "id": "https://example.org/rim/s",
  "type": "RegisterSpecification",
  "title": "Units register specification",
  "definesSubstantiveChange": "Any change to a unit's symbol, dimension or conversion factor is substantive; changes to its description are clarifying.",
  "hasCommitment": [
    {
      "type": "Commitment",
      "commitmentCategory": "persistence",
      "description": "Object identifiers are never reassigned or deleted."
    },
    {
      "type": "Commitment",
      "commitmentCategory": "access",
      "description": "Current and historic content are readable without authentication."
    }
  ]
}
```

#### ttl
```ttl
@prefix dct: <http://purl.org/dc/terms/> .
@prefix ex: <https://example.org/rim/> .
@prefix rim: <https://w3id.org/ogc/rim/> .

ex:s a rim:RegisterSpecification ;
    dct:title "Units register specification" ;
    rim:definesSubstantiveChange "Any change to a unit's symbol, dimension or conversion factor is substantive; changes to its description are clarifying." ;
    rim:hasCommitment [ a rim:Commitment ;
            dct:description "Object identifiers are never reassigned or deleted." ;
            rim:commitmentCategory rim:persistenceCommitment ],
        [ a rim:Commitment ;
            dct:description "Current and historic content are readable without authentication." ;
            rim:commitmentCategory rim:accessCommitment ] .


```


### The same specification in Turtle, attached to its register
#### turtle
```turtle
ex:s a rim:RegisterSpecification ;
    dct:title "Units register specification"@en ;
    rim:definesSubstantiveChange "Any change to a unit's symbol, dimension or conversion factor is substantive; changes to its description are clarifying." ;
    rim:hasCommitment [ a rim:Commitment ; rim:commitmentCategory rim:persistenceCommitment ;
                        dct:description "Object identifiers are never reassigned or deleted."@en ] .

ex:r a rim:Register ;
    rim:hasSpecification ex:s .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Specification
description: The document stating a register's purpose, scope, roles, processes, rules
  and commitments (ISO 19135:2026). `type` is not pinned so that a sub-register may
  use its own term.
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
  title:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/title
  description:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/description
  definesSubstantiveChange:
    description: What counts as a substantive (not merely clarifying) change for this
      register.
    type: string
    x-jsonld-id: https://w3id.org/ogc/rim/definesSubstantiveChange
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
  objectIdentifier:
    x-jsonld-id: https://w3id.org/ogc/rim/objectIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  functionalIdentifier:
    x-jsonld-id: https://w3id.org/ogc/rim/functionalIdentifier
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#anyURI
  runsOn:
    x-jsonld-id: https://w3id.org/ogc/rim/runsOn
    x-jsonld-type: '@id'
  hasSpecification:
    x-jsonld-id: https://w3id.org/ogc/rim/hasSpecification
    x-jsonld-type: '@id'
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
  itemIdentifier: ogc.rim.schemas.register-specification
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-specification/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-specification/schema.yaml)


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
    "objectIdentifier": {
      "@id": "rim:objectIdentifier",
      "@type": "xsd:anyURI"
    },
    "functionalIdentifier": {
      "@id": "rim:functionalIdentifier",
      "@type": "xsd:anyURI"
    },
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
    "title": "dct:title",
    "description": "dct:description",
    "definesSubstantiveChange": "rim:definesSubstantiveChange",
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
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-specification/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/register-specification`

