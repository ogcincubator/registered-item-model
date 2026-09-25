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
