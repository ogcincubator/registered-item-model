# Register Item Class

## Purpose

A **Register Item Class** is a defined abstraction for register items that share common characteristics. It acts as the type or template against which individual registered items are structured and validated.

The distinction is comparable to a class and its instances: the Register Item Class defines what a kind of item is, while Registered Items are the actual managed entries.

## Scope

This block introduces:

- `rim:RegisterItemClass`
- class-level object and functional identifiers
- `rim:definedIn`
- `rim:realizes`
- `rim:validatedBy`

## Conceptual position

```text
Concept
  `-- realized as --> Register Item Class
                         `-- instantiated by --> Register Items
```

## Dependencies

- `register`
- `concept-version`

These dependencies provide the containing register and the concept-plane terms referenced by this block.

## SHACL validation

A Register Item Class requires one object identifier, one functional identifier, at least one title, exactly one containing register and at least one validation resource. It may realize a concept.

## Typical examples

- Dataset class
- Coordinate Reference System class
- API specification class
- Code-list entry class

## Design guidance

Use this block for reusable requirements applying to a category of items. Do not place the values of an individual registered entry here. Those belong in a Registered Item.

## ISO 19135 alignment

This block represents the content-plane abstraction used to define coherent requirements for individual register items.
