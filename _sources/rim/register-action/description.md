# Register Action

## Purpose

A **Register Action** records a governed operation that applies to a Registered Item. Actions make changes explicit and support traceability of how managed register content evolves.

Keeping actions separate from registered items avoids embedding process history directly in the item model and allows action vocabularies to evolve independently.

## Scope

This block introduces:

- `rim:RegisterAction`
- `rim:RegisterActionType`
- `rim:ChangeClassification`
- `rim:actionType`
- `rim:changeClassification`
- `rim:appliesTo`
- `rim:hasAction`

It also uses `prov:startedAtTime` for action timing.

## Conceptual position

```text
Registered Item
  `-- hasAction --> Register Action
                       |-- actionType
                       |-- changeClassification
                       |-- appliesTo --> Registered Item
                       `-- startedAtTime
```

## Dependencies

- `registered-item`

## SHACL validation

A register action requires exactly one action type and exactly one target registered item. It may have one change classification and one start time.

## Typical examples

- addition of a new registered item
- supersession of an existing item
- invalidation or retirement
- publication-state change
- correctional or substantive change

## ISO 19135 alignment

This block provides the structural representation of an action record. Authorization, approval, evidence retention and other governance-process requirements require tests beyond the local SHACL shape.
