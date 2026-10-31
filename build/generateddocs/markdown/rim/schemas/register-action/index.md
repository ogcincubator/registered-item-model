
# Register Action (Schema)

`ogc.rim.schemas.register-action` *v0.2*

A REGISTER interaction activity model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Action

## Purpose

A **Register Action** records a governed operation that applies to a Registered Item. Actions make changes explicit and support traceability of how managed register content evolves.

Keeping actions separate from registered items avoids embedding process history directly in the item model and allows action vocabularies to evolve independently.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterAction` (a `prov:Activity`)
- `rim:actionType` (values: the model's `rim:additionAction` ... `rim:deletionAction`)
- `rim:changeClassification` (values: `rim:substantiveChange`, `rim:clarifyingChange`,
  `rim:correctionalChange`)
- `rim:appliesTo`
- `rim:hasAction`
- `rim:proposer`

It also uses `prov:startedAtTime` / `prov:endedAtTime` for action timing.

## Conceptual position

```text
Registered Item
  `-- hasAction --> Register Action
                       |-- actionType
                       |-- changeClassification
                       |-- appliesTo --> Registered Item
                       `-- startedAtTime
```

## Dependencies

- `registered-item`

## SHACL validation

`rim:RegisterActionShape` (core-ontology `shapes.shacl`): exactly one action type from the model's
scheme; at most one change classification, one target item (a `rim:RegisterItem`) and one start
time.

## Typical examples

- addition of a new registered item
- supersession of an existing item
- invalidation or retirement
- publication-state change
- correctional or substantive change

## ISO 19135 alignment

This block provides the structural representation of an action record. Authorization, approval, evidence retention and other governance-process requirements require tests beyond the local SHACL shape.

## Examples

### The addition of a register item, as proposed and recorded (JSON)
The action that added the "metre" item: its type and change classification come from the
model's own SKOS schemes (`rim:additionAction`, `rim:substantiveChange`); the proposer is a
`prov:Agent`; the item acted upon is embedded with its required core.

#### json
```json
{
  "id": "https://example.org/rim/a",
  "type": "RegisterAction",
  "actionType": "addition",
  "changeClassification": "substantive",
  "proposer": { "id": "https://example.org/org/metrology", "type": "Organization", "label": "Metrology office" },
  "startedAtTime": "2026-09-25T10:00:00Z",
  "appliesTo": {
    "id": "https://example.org/rim/i",
    "type": "RegisterItem",
    "objectIdentifier": "urn:uuid:44444444-4444-4444-8444-444444444444",
    "title": "metre",
    "itemClass": { "id": "https://example.org/rim/uc", "type": "RegisterItemClass", "title": "Unit" },
    "validityStatus": "valid",
    "publicationStatus": "published"
  }
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
    "https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-action/context.jsonld"
  ],
  "id": "https://example.org/rim/a",
  "type": "RegisterAction",
  "actionType": "addition",
  "changeClassification": "substantive",
  "proposer": {
    "id": "https://example.org/org/metrology",
    "type": "Organization",
    "label": "Metrology office"
  },
  "startedAtTime": "2026-09-25T10:00:00Z",
  "appliesTo": {
    "id": "https://example.org/rim/i",
    "type": "RegisterItem",
    "objectIdentifier": "urn:uuid:44444444-4444-4444-8444-444444444444",
    "title": "metre",
    "itemClass": {
      "id": "https://example.org/rim/uc",
      "type": "RegisterItemClass",
      "title": "Unit"
    },
    "validityStatus": "valid",
    "publicationStatus": "published"
  }
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

ex:a a rim:RegisterAction ;
    prov:startedAtTime "2026-09-25T10:00:00+00:00"^^xsd:dateTime ;
    rim:actionType rim:additionAction ;
    rim:appliesTo ex:i ;
    rim:changeClassification rim:substantiveChange ;
    rim:proposer <https://example.org/org/metrology> .

<https://example.org/org/metrology> a prov:Organization ;
    rdfs:label "Metrology office" .

ex:i a rim:RegisterItem ;
    dct:title "metre" ;
    rim:itemClass ex:uc ;
    rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI ;
    rim:publicationStatus rim:published ;
    rim:validityStatus rim:valid .

ex:uc a rim:RegisterItemClass ;
    dct:title "Unit" .


```


### The same action in Turtle
#### turtle
```turtle
ex:a a rim:RegisterAction ;
    rim:actionType rim:additionAction ;
    rim:changeClassification rim:substantiveChange ;
    rim:proposer <https://example.org/org/metrology> ;
    rim:appliesTo ex:i ;
    prov:startedAtTime "2026-09-25T10:00:00Z"^^xsd:dateTime .

<https://example.org/org/metrology> a prov:Organization .

ex:i a rim:RegisterItem ;
    rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI ;
    rim:itemClass ex:uc ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    dct:title "metre"@en .

ex:uc a rim:RegisterItemClass ;
    dct:title "Unit"@en .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Action
description: 'A recorded, governed change to register content (ISO 19135:2026), a
  prov:Activity: what kind of change, how it is classified, which item it applied
  to, who proposed it, when. `type` is not pinned so that a sub-register may use its
  own term.'
type: object
required:
- id
- type
- actionType
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
  actionType:
    enum:
    - addition
    - publish
    - unpublish
    - invalidation
    - supersession
    - redaction
    - deletion
    x-jsonld-id: https://w3id.org/ogc/rim/actionType
    x-jsonld-type: '@vocab'
  changeClassification:
    enum:
    - substantive
    - clarifying
    - correctional
    x-jsonld-id: https://w3id.org/ogc/rim/changeClassification
    x-jsonld-type: '@vocab'
  appliesTo:
    description: The register item acted upon (an IRI or an embedded node).
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/appliesTo
    x-jsonld-type: '@id'
  proposer:
    $ref: '#/$defs/ref'
    x-jsonld-id: https://w3id.org/ogc/rim/proposer
    x-jsonld-type: '@id'
  wasAssociatedWith:
    $ref: '#/$defs/refs'
    x-jsonld-id: http://www.w3.org/ns/prov#wasAssociatedWith
    x-jsonld-type: '@id'
  startedAtTime:
    type: string
    format: date-time
    x-jsonld-id: http://www.w3.org/ns/prov#startedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  endedAtTime:
    type: string
    format: date-time
    x-jsonld-id: http://www.w3.org/ns/prov#endedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  description:
    type: string
    x-jsonld-id: http://purl.org/dc/terms/description
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
  title: http://purl.org/dc/terms/title
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
  addition: https://w3id.org/ogc/rim/additionAction
  publish: https://w3id.org/ogc/rim/publishAction
  unpublish: https://w3id.org/ogc/rim/unpublishAction
  invalidation: https://w3id.org/ogc/rim/invalidationAction
  supersession: https://w3id.org/ogc/rim/supersessionAction
  redaction: https://w3id.org/ogc/rim/redactionAction
  deletion: https://w3id.org/ogc/rim/deletionAction
  substantive: https://w3id.org/ogc/rim/substantiveChange
  clarifying: https://w3id.org/ogc/rim/clarifyingChange
  correctional: https://w3id.org/ogc/rim/correctionalChange
x-jsonld-prefixes:
  rim: https://w3id.org/ogc/rim/
  prov: http://www.w3.org/ns/prov#
  dct: http://purl.org/dc/terms/
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  skos: http://www.w3.org/2004/02/skos/core#
  xsd: http://www.w3.org/2001/XMLSchema#
x-bblocks-metadata:
  itemIdentifier: ogc.rim.schemas.register-action
  register: https://ogcincubator.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-10-31'

```

Links to the schema:

* YAML version: [schema.yaml](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-action/schema.json)
* JSON version: [schema.json](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-action/schema.yaml)


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
    "addition": "rim:additionAction",
    "publish": "rim:publishAction",
    "unpublish": "rim:unpublishAction",
    "invalidation": "rim:invalidationAction",
    "supersession": "rim:supersessionAction",
    "redaction": "rim:redactionAction",
    "deletion": "rim:deletionAction",
    "substantive": "rim:substantiveChange",
    "clarifying": "rim:clarifyingChange",
    "correctional": "rim:correctionalChange",
    "id": "@id",
    "type": "@type",
    "actionType": {
      "@id": "rim:actionType",
      "@type": "@vocab"
    },
    "changeClassification": {
      "@id": "rim:changeClassification",
      "@type": "@vocab"
    },
    "appliesTo": {
      "@id": "rim:appliesTo",
      "@type": "@id"
    },
    "proposer": {
      "@id": "rim:proposer",
      "@type": "@id"
    },
    "wasAssociatedWith": {
      "@id": "prov:wasAssociatedWith",
      "@type": "@id"
    },
    "startedAtTime": {
      "@id": "prov:startedAtTime",
      "@type": "xsd:dateTime"
    },
    "endedAtTime": {
      "@id": "prov:endedAtTime",
      "@type": "xsd:dateTime"
    },
    "description": "dct:description",
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
[context.jsonld](https://ogcincubator.github.io/registered-item-model/build/annotated/rim/schemas/register-action/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/schemas/register-action`

