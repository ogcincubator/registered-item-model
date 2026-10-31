# Register

## Purpose

A **Register** is the managed collection at the centre of the ISO 19135 registration framework. It provides the organizational boundary within which information is identified, maintained and made accessible through governed processes.

This block represents the register itself and the information system on which it is maintained. It deliberately does not define the governance document, registered content, semantic concepts or change actions. Those concerns are provided by separate blocks.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:Register`
- `rim:RegisterSystem`
- register-level object and functional identifiers
- `rim:runsOn`, linking a register to its supporting system
- the six governance roles (`rim:registerOwner`, `rim:registerManager`, `rim:controlBody`,
  `rim:registerSystemManager`, `rim:registerUser`; `rim:proposer` is on actions)

`schema.yaml` lists these as JSON properties; `context.jsonld` maps them onto the ontology. `type`
is deliberately not pinned to `Register`, so that a sub-register can `allOf`-reference this schema
while using its own type term (a sub-class of `rim:Register` in its own ontology).

## Conceptual position

```text
Register
  |-- runsOn --> Register System
  |-- hasSpecification --> Register Specification
  `-- contains governed Register Items
```

The last two relationships are defined by dependent blocks, keeping this base block independent.

## Dependencies

`core-ontology`, which supplies every term and the SHACL shapes.

## SHACL validation

`rim:RegisterShape` (core-ontology `shapes.shacl`): at most one title, at most one object and one
functional identifier (each an `xsd:anyURI` literal), at most one specification and one register
system; commitments, if any, must be `rim:Commitment`s. Nothing is required: the ISO 19135:2026
required core applies to items and concept versions, not to the register node itself.

## Typical uses

- coordinate reference system register
- vocabulary or code-list register
- metadata profile register
- API or specification register

## ISO 19135 alignment

The block implements a technology-specific RDF representation of the ISO 19135 register concept. It is an implementation profile, not a claim that SHACL validation alone establishes full ISO 19135 conformance.
