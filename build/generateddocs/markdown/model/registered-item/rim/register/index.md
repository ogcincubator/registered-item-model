
# Register (Schema)

`ogc.model.registered-item.rim.register` *v0.2*

A REGISTER description model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register

## Purpose

A **Register** is the managed collection at the centre of the ISO 19135 registration framework. It provides the organizational boundary within which information is identified, maintained and made accessible through governed processes.

This block represents the register itself and the information system on which it is maintained. It deliberately does not define the governance document, registered content, semantic concepts or change actions. Those concerns are provided by separate blocks.

## Scope

This block introduces:

- `rim:Register`
- `rim:RegisterSystem`
- register-level object and functional identifiers
- `rim:runsOn`, linking a register to its supporting system

## Conceptual position

```text
Register
  |-- runsOn --> Register System
  |-- hasSpecification --> Register Specification
  `-- contains governed Register Items
```

The last two relationships are defined by dependent blocks, keeping this base block independent.

## Dependencies

None. This is a foundational block.

## SHACL validation

The shape requires one object identifier, one functional identifier and at least one title. A register may have a description and at most one supporting register system.

## Typical uses

- coordinate reference system register
- vocabulary or code-list register
- metadata profile register
- API or specification register

## ISO 19135 alignment

The block implements a technology-specific RDF representation of the ISO 19135 register concept. It is an implementation profile, not a claim that SHACL validation alone establishes full ISO 19135 conformance.

## Examples

### Valid ISO 19135 Register example
#### turtle
```turtle
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix ex: <https://example.org/rim/>.

ex:r a rim:Register;
  rim:objectIdentifier <urn:uuid:11111111-1111-4111-8111-111111111111>;
  rim:functionalIdentifier <https://example.org/registers/units>;
  dct:title "Register of Units"@en.


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:Register
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
  itemIdentifier: ogc.model.registered-item.rim.register
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register/schema.yaml)


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
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/register`

