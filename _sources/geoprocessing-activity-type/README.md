# Geoprocessing Activity Type Profile

A profile of the [Activity Type Register Profile](../activity-type) for registers of
**geoprocessing activity types**: kinds of `prov:Activity` that use or generate spatial data.

## What it adds

- **`geoproc:GeoprocessingActivityType`**: a subclass of `acttype:ActivityType` for specialised types of geoprocessing
- **A constraint on related PROV object types**: every registered sub-class of
  `geoproc:GeoprocessingActivity` must declare at least one `acttype:usedEntityType` or
  `acttype:generatedEntityType` that is a spatial data type. A spatial data type is a class that
  is, or specialises, a GeoSPARQL 1.1 `geo:SpatialObject` or `geo:SpatialObjectCollection`
  (`geo:Feature`, `geo:Geometry`, `geo:FeatureCollection`, …). "Used **or** generated" allows
  activities such as model training, which consume spatial data but produce a non-spatial
  artefact.

Sub-classes that are only declared in an ontology and are not registered as
`acttype:ActivityType` items are not checked. See the Activity Type Register Profile README for
the general pattern.

## Profiles of this profile

The [ML Activity Types Profile](../ml-activity-profile) specialises `geoproc:GeoprocessingActivity` with
`mlact:MLActivity` and adds constraints of its own for machine learning activity types.
