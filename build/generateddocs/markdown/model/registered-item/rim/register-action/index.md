
# Register Action (Schema)

`ogc.model.registered-item.rim.register-action` *v0.2*

A REGISTER interaction activity model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Action

## Purpose

A **Register Action** records a governed operation that applies to a Registered Item. Actions make changes explicit and support traceability of how managed register content evolves.

Keeping actions separate from registered items avoids embedding process history directly in the item model and allows action vocabularies to evolve independently.

## Scope

This block introduces:

- `rim:RegisterAction`
- `rim:RegisterActionType`
- `rim:ChangeClassification`
- `rim:actionType`
- `rim:changeClassification`
- `rim:appliesTo`
- `rim:hasAction`

It also uses `prov:startedAtTime` for action timing.

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

A register action requires exactly one action type and exactly one target registered item. It may have one change classification and one start time.

## Typical examples

- addition of a new registered item
- supersession of an existing item
- invalidation or retirement
- publication-state change
- correctional or substantive change

## ISO 19135 alignment

This block provides the structural representation of an action record. Authorization, approval, evidence retention and other governance-process requirements require tests beyond the local SHACL shape.

## Examples

### Valid ISO 19135 Register Action example
#### turtle
```turtle
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix prov: <http://www.w3.org/ns/prov#>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix ex: <https://example.org/rim/>.
@prefix skos: <http://www.w3.org/2004/02/skos/core#>.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.

ex:a a rim:RegisterAction;
  rim:actionType ex:addition;
  rim:appliesTo ex:i;
  prov:startedAtTime "2026-09-25T10:00:00Z"^^xsd:dateTime.

ex:addition a rim:RegisterActionType.

ex:i a rim:RegisterItem;
  rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/items/metre"^^xsd:anyURI;
  rim:itemClass ex:uc;
  rim:inRegister ex:r;
  rim:realizes ex:cv;
  rim:validityStatus ex:valid;
  rim:publicationStatus ex:published;
  dct:title "metre"@en.

ex:valid a rim:ValidityStatus .

ex:published a rim:PublicationStatus .

ex:uc a rim:RegisterItemClass;
  rim:objectIdentifier "urn:uuid:33333333-3333-4333-8333-333333333333"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/classes/unit"^^xsd:anyURI;
  dct:title "Unit"@en;
  rim:definedIn ex:r;
  rim:realizes ex:c;
  rim:validatedBy "https://example.org/rules/unit"^^xsd:anyURI.

ex:r a rim:Register;
  rim:objectIdentifier "urn:uuid:11111111-1111-4111-8111-111111111111"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/registers/units"^^xsd:anyURI;
  dct:title "Register of Units"@en.

ex:cv a rim:ConceptVersion;
  rim:versionOf ex:c;
  rim:version "1.0";
  rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/concepts/metre/1.0"^^xsd:anyURI;
  rim:validityStatus ex:valid;
  rim:publicationStatus ex:published.

ex:c a rim:Concept;
  skos:prefLabel "metre"@en .
```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Action
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:RegisterAction
    x-jsonld-id: '@type'
required:
- id
- type
additionalProperties: true
x-jsonld-prefixes:
  rim: https://w3id.org/ogc/rim/
  dct: http://purl.org/dc/terms/
  skos: http://www.w3.org/2004/02/skos/core#
  prov: http://www.w3.org/ns/prov#
x-bblocks-metadata:
  itemIdentifier: ogc.model.registered-item.rim.register-action
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-action/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-action/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "id": "@id",
    "type": "@type",
    "rim": "https://w3id.org/ogc/rim/",
    "dct": "http://purl.org/dc/terms/",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "prov": "http://www.w3.org/ns/prov#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-action/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/register-action`

