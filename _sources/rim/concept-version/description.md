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
