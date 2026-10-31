
# Concept Version (Schema)

`ogc.rim.schemas.concept-version` *v0.2*

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Concept Version

## Purpose

A **Concept Version** represents the meaning of a concept at a particular point in time. It belongs to the concept plane, which allows semantic meaning to evolve independently from the concrete register items used to represent it.

A concept is the stable semantic identity. A concept version captures a particular state of that meaning, together with its version and administrative statuses.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:Concept`
- `rim:ConceptVersion`
- `rim:versionOf`
- `rim:version`
- `rim:validityStatus`, `rim:publicationStatus` (values: the model's own `rim:valid` / `rim:invalid`,
  `rim:published` / `rim:unpublished`)
- concept-version object and functional identifiers

## Conceptual position

```text
Concept
  `-- has version --> Concept Version
                        `-- is realized by --> Register Item
```

The bridge to a Register Item is owned by the Registered Item block, avoiding a dependency cycle.

## Dependencies

`core-ontology`, which supplies every term and the SHACL shapes. The concept plane is otherwise
independently reusable.

## SHACL validation

`rim:ConceptShape` and `rim:ConceptVersionShape` (core-ontology `shapes.shacl`): a concept requires
at least one preferred label; a concept version requires exactly one parent concept, one object
identifier, one validity and one publication status, and may carry at most one version string and
one functional identifier.

## Example

```text
Concept: Dataset
Concept Version: Dataset 3.0
Possible realization: a registered dataset record conforming to that semantic version
```

## ISO 19135 alignment

This block captures the ISO 19135 separation between semantic identity and concrete managed content. Lifecycle transitions and permitted status combinations may require additional register-specific rules.

## Examples

### A concept and one version of it (JSON)
The concept "metre" and its version 1.0. The version, not the concept, carries validity and
publication status, using the model's own values (`rim:valid`, `rim:published`).

#### json
```json
{
  "id": "https://example.org/rim/cv",
  "type": "ConceptVersion",
  "versionOf": { "id": "https://example.org/rim/c", "type": "Concept", "prefLabel": "metre" },
  "version": "1.0",
  "objectIdentifier": "urn:uuid:22222222-2222-4222-8222-222222222222",
  "functionalIdentifier": "https://example.org/concepts/metre/1.0",
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
      "skos": "http://www.w3.org/2004/02/skos/core#",
      "xsd": "http://www.w3.org/2001/XMLSchema#",
      "ex": "https://example.org/rim/"
    },
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/concept-version/context.jsonld"
  ],
  "id": "https://example.org/rim/cv",
  "type": "ConceptVersion",
  "versionOf": {
    "id": "https://example.org/rim/c",
    "type": "Concept",
    "prefLabel": "metre"
  },
  "version": "1.0",
  "objectIdentifier": "urn:uuid:22222222-2222-4222-8222-222222222222",
  "functionalIdentifier": "https://example.org/concepts/metre/1.0",
  "validityStatus": "valid",
  "publicationStatus": "published"
}
```

#### ttl
```ttl
@prefix ex: <https://example.org/rim/> .
@prefix rim: <https://w3id.org/ogc/rim/> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:cv a rim:ConceptVersion ;
    rim:functionalIdentifier "https://example.org/concepts/metre/1.0"^^xsd:anyURI ;
    rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI ;
    rim:publicationStatus rim:published ;
    rim:validityStatus rim:valid ;
    rim:version "1.0" ;
    rim:versionOf ex:c .

ex:c a rim:Concept ;
    skos:prefLabel "metre" .


```


### The same concept version in Turtle
#### turtle
```turtle
ex:c a rim:Concept ;
    skos:prefLabel "metre"@en .

ex:cv a rim:ConceptVersion ;
    rim:versionOf ex:c ;
    rim:version "1.0" ;
    rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI ;
    rim:functionalIdentifier "https://example.org/concepts/metre/1.0"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Concept Version
description: A version of a concept, in a register's concept plane. Concept versions,
  not concepts, carry the required validity and publication statuses (ISO 19135:2026).
  `versionOf` is the concept, as an IRI or an embedded rim:Concept node (`prefLabel`
  required there). `type` is not pinned so that a sub-register may use its own term.
type: object
required:
- id
- type
- versionOf
- objectIdentifier
- validityStatus
- publicationStatus
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
  versionOf:
    oneOf:
    - type: string
      format: uri
    - type: object
      required:
      - id
      - prefLabel
      properties:
        id:
          type: string
          format: uri
          x-jsonld-id: '@id'
        type:
          const: Concept
          x-jsonld-id: '@type'
        prefLabel:
          type: string
          x-jsonld-id: http://www.w3.org/2004/02/skos/core#prefLabel
        definition:
          type: string
          x-jsonld-id: http://www.w3.org/2004/02/skos/core#definition
        inScheme:
          type: string
          format: uri
          x-jsonld-id: http://www.w3.org/2004/02/skos/core#inScheme
          x-jsonld-type: '@id'
    x-jsonld-id: https://w3id.org/ogc/rim/versionOf
    x-jsonld-type: '@id'
  version:
    type: string
    x-jsonld-id: https://w3id.org/ogc/rim/version
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
  title: http://purl.org/dc/terms/title
  description: http://purl.org/dc/terms/description
  label: http://www.w3.org/2000/01/rdf-schema#label
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
  itemClass:
    x-jsonld-id: https://w3id.org/ogc/rim/itemClass
    x-jsonld-type: '@id'
  inRegister:
    x-jsonld-id: https://w3id.org/ogc/rim/inRegister
    x-jsonld-type: '@id'
  realizes:
    x-jsonld-id: https://w3id.org/ogc/rim/realizes
    x-jsonld-type: '@id'
  valid: https://w3id.org/ogc/rim/valid
  invalid: https://w3id.org/ogc/rim/invalid
  published: https://w3id.org/ogc/rim/published
  unpublished: https://w3id.org/ogc/rim/unpublished
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
  itemIdentifier: ogc.rim.schemas.concept-version
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/concept-version/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/concept-version/schema.yaml)


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
    "title": "dct:title",
    "description": "dct:description",
    "label": "rdfs:label",
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
    "valid": "rim:valid",
    "invalid": "rim:invalid",
    "published": "rim:published",
    "unpublished": "rim:unpublished",
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
    "versionOf": {
      "@context": {
        "prefLabel": "skos:prefLabel",
        "definition": "skos:definition",
        "inScheme": {
          "@id": "skos:inScheme",
          "@type": "@id"
        }
      },
      "@id": "rim:versionOf",
      "@type": "@id"
    },
    "version": "rim:version",
    "objectIdentifier": {
      "@id": "rim:objectIdentifier",
      "@type": "xsd:anyURI"
    },
    "functionalIdentifier": {
      "@id": "rim:functionalIdentifier",
      "@type": "xsd:anyURI"
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
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/concept-version/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/concept-version`

