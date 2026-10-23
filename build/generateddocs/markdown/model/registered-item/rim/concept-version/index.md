
# Concept Version (Schema)

`ogc.model.registered-item.rim.concept-version` *v0.2*

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

[*Status*](http://www.opengis.net/def/status): Under development

## Description

# Concept Version

## Purpose

A **Concept Version** represents the meaning of a concept at a particular point in time. It belongs to the concept plane, which allows semantic meaning to evolve independently from the concrete register items used to represent it.

A concept is the stable semantic identity. A concept version captures a particular state of that meaning, together with its version and administrative statuses.

## Scope

This block introduces:

- `rim:Concept`
- `rim:ConceptVersion`
- `rim:ValidityStatus`
- `rim:PublicationStatus`
- `rim:versionOf`
- `rim:version`
- `rim:validityStatus`
- `rim:publicationStatus`
- concept-version object and functional identifiers

## Conceptual position

```text
Concept
  `-- has version --> Concept Version
                        `-- is realized by --> Register Item
```

The bridge to a Register Item is owned by the Registered Item block, avoiding a dependency cycle.

## Dependencies

None. The concept plane is independently reusable.

## SHACL validation

A concept requires at least one preferred label. A concept version requires exactly one parent concept, version value, object identifier, functional identifier, validity status and publication status.

## Example

```text
Concept: Dataset
Concept Version: Dataset 3.0
Possible realization: a registered dataset record conforming to that semantic version
```

## ISO 19135 alignment

This block captures the ISO 19135 separation between semantic identity and concrete managed content. Lifecycle transitions and permitted status combinations may require additional register-specific rules.

## Examples

### Valid ISO 19135 Concept Version example
#### turtle
```turtle

@prefix rim: <https://w3id.org/ogc/rim/>.
@prefix skos: <http://www.w3.org/2004/02/skos/core#>.
@prefix ex: <https://example.org/rim/>.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.

ex:c a rim:Concept;
  skos:prefLabel "metre"@en.

ex:cv a rim:ConceptVersion;
  rim:versionOf ex:c;
  rim:version "1.0";
  rim:objectIdentifier "urn:uuid:22222222-2222-4222-8222-222222222222"^^xsd:anyURI;
  rim:functionalIdentifier "https://example.org/concepts/metre/1.0"^^xsd:anyURI;
  rim:validityStatus ex:valid;
  rim:publicationStatus ex:published.

ex:valid a rim:ValidityStatus.

ex:published a rim:PublicationStatus.


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: ISO 19135 Concept Version
type: object
properties:
  id:
    type: string
    format: uri
    x-jsonld-id: '@id'
  type:
    const: rim:ConceptVersion
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
  itemIdentifier: ogc.model.registered-item.rim.concept-version
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.2'
  dateOfLastChange: '2026-09-25'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/concept-version/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/concept-version/schema.yaml)


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
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/concept-version/context.jsonld)


# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/rim/concept-version`

