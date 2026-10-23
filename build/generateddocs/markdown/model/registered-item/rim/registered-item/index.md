
# Registered Item (Schema)

`ogc.model.registered-item.rim.registered-item` *v0.2*

A REGISTER ITEM model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. This provides a canonical schema for a register entry, that can be extended according the REGISTER ITEM CLASS defined for the register.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Registered Item

## Purpose

A **Registered Item** is an actual managed unit of information in a register. It is an instance of a Register Item Class and can realize a Concept Version from the concept plane.

This block represents operational register content rather than the template that constrains that content.

## Scope

This block introduces:

- `rim:RegisterItem`
- item-level object and functional identifiers
- `rim:itemClass`
- `rim:inRegister`
- `rim:realizes`
- `rim:relatedItem`
- `rim:supersededBy`
- `rim:supersedes`

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

A registered item requires one object identifier, one functional identifier, exactly one item class and exactly one containing register. It may realize a concept version and may be related to or supersede other registered items.

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

### Valid ISO 19135 Registered Item example
#### turtle
```turtle
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix skos: <http://www.w3.org/2004/02/skos/core#>.
@prefix ex: <https://example.org/rim/>.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.

ex:i a rim:RegisterItem;
  rim:objectIdentifier "urn:uuid:44444444-4444-4444-8444-444444444444"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/items/metre"^^xsd:anyURI;
  rim:itemClass ex:uc;
  rim:inRegister ex:r;
  rim:realizes ex:cv;
  rim:validityStatus ex:valid;
  rim:publicationStatus ex:published;
  dct:title "metre"@en.

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

ex:valid a rim:ValidityStatus .

ex:published a rim:PublicationStatus .

ex:c a rim:Concept;
  skos:prefLabel "metre"@en .
```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Registered Item
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:RegisterItem
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
  itemIdentifier: ogc.model.registered-item.rim.registered-item
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/registered-item/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/registered-item/schema.yaml)


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
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/registered-item/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/registered-item`

