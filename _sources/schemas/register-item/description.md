# Registered Item

## Purpose

A **Registered Item** is an actual managed unit of information in a register. It is an instance of a Register Item Class and can realize a Concept Version from the concept plane.

This block represents operational register content rather than the template that constrains that content.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterItem`
- item-level object and functional identifiers
- `rim:itemClass`
- `rim:inRegister`
- `rim:realizes`
- `rim:validityStatus`, `rim:publicationStatus`, `rim:isRedacted`, `rim:isDeleted`
- `rim:relatedItem`
- `rim:supersededBy`
- `rim:supersedes`
- `rim:hasAction`

`schema.yaml` requires exactly the ISO 19135:2026 required core -- `objectIdentifier`,
`itemClass`, `validityStatus`, `publicationStatus` -- and nothing else, so that a sub-register can
`allOf`-reference it and add its own content properties. `type` is deliberately not pinned to
`RegisterItem`: a sub-register uses its own type term, declared in its ontology as a sub-class of
`rim:RegisterItem`, and the base shape then applies to its items through the ontology closure.

Status values are the model's own SKOS concepts: `valid` / `invalid` map to `rim:valid` /
`rim:invalid`, `published` / `unpublished` to `rim:published` / `rim:unpublished`. A register must
not mint its own.

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

`rim:RegisterItemShape` (core-ontology `shapes.shacl`): exactly one object identifier
(`xsd:anyURI` literal), exactly one item class that is a `rim:RegisterItemClass`, exactly one
validity and one publication status drawn from the model's values; at most one functional
identifier, one register (a `rim:Register`) and one realised concept version. Related, superseding
and superseded items must themselves be `rim:RegisterItem`s.

`tests/item-class-untyped-fail.json` is the negative case: it passes the JSON Schema and is
rejected because its item class is not a declared `rim:RegisterItemClass`.

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
