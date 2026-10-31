# Register Action

## Purpose

A **Register Action** records a governed operation that applies to a Registered Item. Actions make changes explicit and support traceability of how managed register content evolves.

Keeping actions separate from registered items avoids embedding process history directly in the item model and allows action vocabularies to evolve independently.

## Scope

This block is the JSON / JSON-LD binding of the following core-ontology terms (defined once, in
`ogc.model.registered-item.core-ontology`, not redeclared here):

- `rim:RegisterAction` (a `prov:Activity`)
- `rim:actionType` (values: the model's `rim:additionAction` ... `rim:deletionAction`)
- `rim:changeClassification` (values: `rim:substantiveChange`, `rim:clarifyingChange`,
  `rim:correctionalChange`)
- `rim:appliesTo`
- `rim:hasAction`
- `rim:proposer`

It also uses `prov:startedAtTime` / `prov:endedAtTime` for action timing.

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

`rim:RegisterActionShape` (core-ontology `shapes.shacl`): exactly one action type from the model's
scheme; at most one change classification, one target item (a `rim:RegisterItem`) and one start
time.

## Typical examples

- addition of a new registered item
- supersession of an existing item
- invalidation or retirement
- publication-state change
- correctional or substantive change

## ISO 19135 alignment

This block provides the structural representation of an action record. Authorization, approval, evidence retention and other governance-process requirements require tests beyond the local SHACL shape.
