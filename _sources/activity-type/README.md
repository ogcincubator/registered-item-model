# Activity Type Register Profile

A profile of the [Registered Item Model](../core-ontology) for registers whose items are **registered activity types**: kinds of activity such as a quality review, a reprojection or a model
inference run, governed with the same identifiers, statuses and change history as any other
register item.

## What it adds

- **`acttype:ActivityType`**: a subclass of `rim:RegisterItem`. Specific activity type classes
  specialise this root. Registered types can be declared as classes and used to type individual
  activities. Those activities also inherit the register item requirements, so they need their
  own identifiers, statuses and `acttype:activityTypeItemClass`.
- **A narrowed `rim:itemClass` domain**: the register item class `acttype:activityTypeItemClass`,
  plus SHACL in both directions. Every activity type record must use that item class and belong
  to the `acttype:ActivityType` hierarchy through its `rdf:type`. Every register item using that item class must be an
  `acttype:ActivityType`.
- **Related PROV object types**: type-level counterparts of the PROV relations:
  `acttype:usedEntityType` (`prov:used`), `acttype:generatedEntityType` (`prov:generated`),
  `acttype:associatedAgentType` (`prov:wasAssociatedWith`) and `acttype:informedByActivityType`
  (`prov:wasInformedBy`). Values are classes.
- **An optional plan**: `acttype:plan` (at most one), pointing to a `prov:Plan` that describes the
  steps an activity of this type must carry out. Use [P-Plan](https://www.opmw.org/model/p-plan/)
  for the steps (`p-plan:Step`, `p-plan:isStepOfPlan`, `p-plan:isPrecededBy`). An individual
  activity records the plan it followed with PROV's qualified association
  (`prov:qualifiedAssociation` / `prov:hadPlan`).

## Profiling this block further

A further profile, such as [Geoprocessing Activity Types](../geoprocessing-activity-types), typically:

1. declares a root activity class (for example `geoproc:GeoprocessingActivityType rdfs:subClassOf
   acttype:ActivityType`) that its registered types specialise;
2. constrains the related types that those registered types must declare, with a shape targeting
   every subclass of the root that is also a registered `acttype:ActivityType`:

```turtle
ex:MyRootTypesShape a sh:NodeShape ;
    sh:targetNode ex:MyRootActivity ;
    sh:property [
        sh:path [ sh:oneOrMorePath [ sh:inversePath rdfs:subClassOf ] ] ;
        sh:node [ sh:or ( [ sh:not [ sh:class acttype:ActivityType ] ]
                          ex:MyActivityTypeConstraints ) ] ;
    ] .
```

The `sh:not` branch skips subclasses that are only declared in an ontology and are not being
registered. `ex:MyActivityTypeConstraints` then constrains `acttype:usedEntityType`,
`acttype:generatedEntityType` and similar properties, for example with `sh:qualifiedValueShape`
over an `rdfs:subClassOf*` path.

Each constraining profile can itself be profiled: a subtype's shapes add to its parent's and never
replace them.
