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
