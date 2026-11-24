# Geoprocessing Activity Type Profile

A profile of the [Activity Type Register Profile](../activity-type) for registers of
**geoprocessing activity types**: kinds of `acttype:ActivityType` that use or generate spatial data.

## What it adds

- **`geoproc:GeoprocessingActivityType`**: a subclass of `acttype:ActivityType` for specialized types of geoprocessing
- **A constraint on related PROV object types**: every registered subclass of
  `geoproc:GeoprocessingActivityType` must declare at least one `acttype:usedEntityType` or
  `acttype:generatedEntityType` that is a spatial data type. A spatial data type is a class that
  is, or specializes, a GeoSPARQL 1.1 `geo:SpatialObject` or `geo:SpatialObjectCollection`
  (`geo:Feature`, `geo:Geometry`, `geo:FeatureCollection`, …). "Used **or** generated" allows
  activities such as model training, which consume spatial data but produce a non-spatial
  artifact.

Subclasses that are only declared in an ontology and are not registered as
`acttype:ActivityType` items are not checked. See the Activity Type Register Profile README for
the general pattern.

## Profiles of this profile

The [ML Activity Types](../ml-activity-types) profile specialises `geoproc:GeoprocessingActivityType` with
`mlact:MLActivityType` and adds constraints of its own for machine learning activity types.
