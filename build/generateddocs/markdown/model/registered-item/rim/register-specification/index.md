
# Register Specification (Schema)

`ogc.model.registered-item.rim.register-specification` *v0.2*

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Register Specification

## Purpose

A **Register Specification** documents the governance and requirements of a register and its contents. It explains what the register manages, how identifiers and versions are assigned, which actions and relations are allowed, and which commitments the register makes to its users.

Separating this block from the Register block keeps the managed container distinct from the governance rules that control it.

## Scope

This block introduces:

- `rim:RegisterSpecification`
- `rim:Commitment`
- `rim:CommitmentCategory`
- `rim:hasSpecification`
- `rim:hasCommitment`
- `rim:commitmentCategory`
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

A register specification must have a title. It may have descriptions, a definition of substantive change and one or more commitments. Each commitment requires a category and a description.

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

### Valid ISO 19135 Register Specification example
#### turtle
```turtle
@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix dct: <http://purl.org/dc/terms/>.
@prefix ex: <https://example.org/rim/>.

ex:s a rim:RegisterSpecification;
  dct:title "Units register specification"@en.

ex:r a rim:Register;
  rim:hasSpecification ex:s.


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Register Specification
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:RegisterSpecification
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
  itemIdentifier: ogc.model.registered-item.rim.register-specification
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-specification/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-specification/schema.yaml)


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
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-specification/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/register-specification`

