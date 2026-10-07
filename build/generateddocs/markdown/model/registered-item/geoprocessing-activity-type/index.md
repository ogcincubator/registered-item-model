
# Geoprocessing Activity Types Profile (Model)

`ogc.model.registered-item.geoprocessing-activity-type` *v0.1*

A profile of the Activity Type Register Profile for geoprocessing activity types: registered sub-classes of geoproc:GeoprocessingActivity, each required to use or generate at least one GeoSPARQL spatial data type.

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### A registered NDVI computation activity type
An Earth observation processing register governs the geoprocessing activity types its
pipelines run. "NDVI computation" specialises `geoproc:GeoprocessingActivity`. It uses a
multispectral scene and generates an NDVI raster, both sub-classes of `geo:Feature`, which
satisfies the profile's requirement for at least one spatial used or generated entity type.
Its plan lists the three steps every NDVI computation must perform, in order.

#### turtle
```turtle
ex:eoProcessingRegister a rim:Register ;
    dct:title "EO Processing Activity Types" .

ex:NdviComputation a acttype:ActivityType , owl:Class ;
    rdfs:subClassOf geoproc:GeoprocessingActivity ;
    rdfs:label "NDVI computation" ;
    dct:title "NDVI computation" ;
    dct:description "Computes the Normalized Difference Vegetation Index from the red and near-infrared bands of a multispectral scene." ;
    rim:inRegister ex:eoProcessingRegister ;
    rim:itemClass acttype:activityTypeItemClass ;
    rim:objectIdentifier "https://example.org/registers/geoprocessing/ndvi-computation/v1"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    acttype:usedEntityType ex:MultispectralScene ;
    acttype:generatedEntityType ex:NdviRaster ;
    acttype:associatedAgentType prov:SoftwareAgent ;
    acttype:plan ex:ndviPlan .

ex:MultispectralScene a owl:Class ;
    rdfs:subClassOf geo:Feature , prov:Entity ;
    rdfs:label "Multispectral scene" .

ex:NdviRaster a owl:Class ;
    rdfs:subClassOf geo:Feature , prov:Entity ;
    rdfs:label "NDVI raster" .

ex:ndviPlan a prov:Plan , p-plan:Plan ;
    dct:title "NDVI computation procedure" .

ex:cloudMask a p-plan:Step ;
    p-plan:isStepOfPlan ex:ndviPlan ;
    dct:description "Mask cloud and cloud-shadow pixels using the scene's quality band." .

ex:reproject a p-plan:Step ;
    p-plan:isStepOfPlan ex:ndviPlan ;
    p-plan:isPrecededBy ex:cloudMask ;
    dct:description "Reproject the red and near-infrared bands to the target grid." .

ex:computeIndex a p-plan:Step ;
    p-plan:isStepOfPlan ex:ndviPlan ;
    p-plan:isPrecededBy ex:reproject ;
    dct:description "Compute (NIR - Red) / (NIR + Red) per pixel." .

```

## Sources

* [OGC GeoSPARQL 1.1](https://docs.ogc.org/is/22-047r1/22-047r1.html)
* [W3C PROV-O](https://www.w3.org/TR/prov-o/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/geoprocessing-activity-type`

