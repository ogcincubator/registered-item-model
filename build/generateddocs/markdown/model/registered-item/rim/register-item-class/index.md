
# Register Item Class (Schema)

`ogc.model.registered-item.rim.register-item-class` *v0.2*

A REGISTER ITEM CLASS model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. (describes the type of object in a register) Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Item Class

## Purpose

A **Register Item Class** is a defined abstraction for register items that share common characteristics. It acts as the type or template against which individual registered items are structured and validated.

The distinction is comparable to a class and its instances: the Register Item Class defines what a kind of item is, while Registered Items are the actual managed entries.

## Scope

This block introduces:

- `rim:RegisterItemClass`
- class-level object and functional identifiers
- `rim:definedIn`
- `rim:realizes`
- `rim:validatedBy`

## Conceptual position

```text
Concept
  `-- realized as --> Register Item Class
                         `-- instantiated by --> Register Items
```

## Dependencies

- `register`
- `concept-version`

These dependencies provide the containing register and the concept-plane terms referenced by this block.

## SHACL validation

A Register Item Class requires one object identifier, one functional identifier, at least one title, exactly one containing register and at least one validation resource. It may realize a concept.

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

### Valid ISO 19135 Register Item Class example
#### turtle
```turtle
@prefix skos: <http://www.w3.org/2004/02/skos/core#>.
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix ex: <https://example.org/rim/>.

ex:uc a rim:RegisterItemClass;
  rim:objectIdentifier <urn:uuid:33333333-3333-4333-8333-333333333333>;
  rim:functionalIdentifier <https://example.org/classes/unit>;
  dct:title "Unit"@en;
  rim:definedIn ex:r;
  rim:realizes ex:c;
  rim:validatedBy <https://example.org/rules/unit>.

ex:r a rim:Register.

ex:c a rim:Concept ;
  skos:prefLabel "metre"@en
.


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Item Class
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:RegisterItemClass
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
  itemIdentifier: ogc.model.registered-item.rim.register-item-class
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-item-class/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-item-class/schema.yaml)


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
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-item-class/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/register-item-class`

