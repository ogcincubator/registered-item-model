# Register Item Class

## Purpose

A **Register Item Class** is a defined abstraction for register items that share common characteristics. It acts as the type or template against which individual registered items are structured and validated.

The distinction is comparable to a class and its instances: the Register Item Class defines what a kind of item is, while Registered Items are the actual managed entries.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterItemClass`
- class-level object and functional identifiers
- `rim:definedIn`
- `rim:validatedBy`

`rim:realizes` is **not** a property of an item class: its domain is `rim:RegisterItem` and its range
`rim:ConceptVersion`. Whether an item class should relate to a concept of its own is an open
question for the model; until it is settled no such property is offered here.

## Conceptual position

```text
Register
  `-- definedIn (inverse) --> Register Item Class
                                 `-- instantiated by --> Register Items
```

## Dependencies

- `register`
- `concept-version`

These dependencies provide the containing register and the concept-plane terms referenced by this block.

## SHACL validation

`rim:RegisterItemClassShape` (core-ontology `shapes.shacl`): exactly one title; at most one object
and one functional identifier (`xsd:anyURI` literals), at most one defining register, which must be
a `rim:Register`. `validatedBy` is unconstrained in form (an IRI naming the rule set).

## Typical examples

- Dataset class
- Coordinate Reference System class
- API specification class
- Code-list entry class

## Design guidance

Use this block for reusable requirements applying to a category of items. Do not place the values of an individual registered entry here. Those belong in a Registered Item.

## ISO 19135 alignment

This block represents the content-plane abstraction used to define coherent requirements for individual register items.
