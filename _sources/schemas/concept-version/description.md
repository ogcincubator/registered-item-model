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
