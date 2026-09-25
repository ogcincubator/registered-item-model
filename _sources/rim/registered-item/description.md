# Registered Item

## Purpose

A **Registered Item** is an actual managed unit of information in a register. It is an instance of a Register Item Class and can realize a Concept Version from the concept plane.

This block represents operational register content rather than the template that constrains that content.

## Scope

This block introduces:

- `rim:RegisterItem`
- item-level object and functional identifiers
- `rim:itemClass`
- `rim:inRegister`
- `rim:realizes`
- `rim:relatedItem`
- `rim:supersededBy`
- `rim:supersedes`

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

A registered item requires one object identifier, one functional identifier, exactly one item class and exactly one containing register. It may realize a concept version and may be related to or supersede other registered items.

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
