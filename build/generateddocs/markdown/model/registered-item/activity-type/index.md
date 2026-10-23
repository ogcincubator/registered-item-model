
# Activity Type Register Profile (Model)

`ogc.model.registered-item.activity-type` *v0.3*

A profile of the Registered Item Model for governed activity type records in the acttype:ActivityType hierarchy, with related PROV object types and an optional plan.

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### A registered activity type with a plan, and an activity of that type
A data curation register governs the kinds of activity its curators perform. The
"dataset quality review" type is a register item — with its own identifier, status and
item class — and also a class, a sub-class of `acttype:ActivityType`. It declares the kinds of
entity it uses and generates and the kind of agent that performs it, and points to a
P-Plan `prov:Plan` listing its three required steps in order. The last part shows an
individual review typed directly with the registered class, recording the plan it followed
through a PROV qualified association.

#### turtle
```turtle
ex:curationRegister a rim:Register ;
    dct:title "Data Curation Activity Types" .

ex:DatasetQualityReview a acttype:ActivityType , owl:Class ;
    rdfs:subClassOf acttype:ActivityType ;
    rdfs:label "Dataset quality review" ;
    dct:title "Dataset quality review" ;
    dct:description "A review of a dataset against the curation quality criteria, producing a quality report." ;
    rim:inRegister ex:curationRegister ;
    rim:itemClass acttype:activityTypeItemClass ;
    rim:objectIdentifier "https://example.org/registers/activity-types/dataset-quality-review/v1"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    acttype:usedEntityType dcat:Dataset ;
    acttype:generatedEntityType ex:QualityReport ;
    acttype:associatedAgentType prov:Person ;
    acttype:plan ex:qualityReviewPlan .

ex:QualityReport a owl:Class ;
    rdfs:subClassOf prov:Entity .

ex:qualityReviewPlan a prov:Plan , p-plan:Plan ;
    dct:title "Dataset quality review procedure" .

ex:checkCompleteness a p-plan:Step ;
    p-plan:isStepOfPlan ex:qualityReviewPlan ;
    dct:description "Check that every mandatory metadata element is present." .

ex:checkConsistency a p-plan:Step ;
    p-plan:isStepOfPlan ex:qualityReviewPlan ;
    p-plan:isPrecededBy ex:checkCompleteness ;
    dct:description "Check values against the dataset's declared schema and code lists." .

ex:writeReport a p-plan:Step ;
    p-plan:isStepOfPlan ex:qualityReviewPlan ;
    p-plan:isPrecededBy ex:checkConsistency ;
    dct:description "Record the findings in a quality report." .

# An individual activity of the registered type.
ex:review-2026-09-20 a ex:DatasetQualityReview ;
    rim:itemClass acttype:activityTypeItemClass ;
    rim:objectIdentifier "https://example.org/registers/activity-types/reviews/review-2026-09-20"^^xsd:anyURI ;
    rim:validityStatus rim:valid ;
    rim:publicationStatus rim:published ;
    prov:used ex:riverGaugesDataset ;
    prov:wasAssociatedWith ex:curatorJo ;
    prov:qualifiedAssociation [
        a prov:Association ;
        prov:agent ex:curatorJo ;
        prov:hadPlan ex:qualityReviewPlan
    ] ;
    prov:startedAtTime "2026-09-20T09:00:00Z"^^xsd:dateTime .

ex:riverGaugesDataset a dcat:Dataset , prov:Entity .
ex:curatorJo a prov:Person .

ex:report-2026-09-20 a ex:QualityReport , prov:Entity ;
    prov:wasGeneratedBy ex:review-2026-09-20 .

```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Activity Type
description: 'A registered activity type based on the Registered Item Model.

  '
allOf:
- $ref: https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/rim/register-item-class/schema.yaml
- type: object
  properties:
    usedEntityType:
      type: string
      format: uri
      description: "A type of prov:Entity that activities of this type use (prov:used)
        \u2014 for example a type of input dataset."
    generatedEntityType:
      type: string
      format: uri
      description: "A type of prov:Entity that activities of this type generate (prov:generated)
        \u2014 for example a type of output product."
    associatedAgentType:
      type: string
      format: uri
      description: A type of prov:Agent (for example prov:Person, prov:SoftwareAgent,
        or a narrower role-bearing type) that activities of this type are associated
        with (prov:wasAssociatedWith).
    informedByActivityType:
      type: string
      format: uri
      description: "A type of acttype:ActivityType whose instances typically inform
        activities of this type (prov:wasInformedBy) \u2014 for example an upstream
        preparation activity type."
  unevaluatedProperties: true
x-bblocks-metadata:
  itemIdentifier: ogc.model.registered-item.activity-type
  register: https://nielshoffmann.github.io/registered-item-model/build/register.json
  version: '0.3'
  dateOfLastChange: '2026-10-23'

```

Links to the schema:

* YAML version: [schema.yaml](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/activity-type/schema.json)
* JSON version: [schema.json](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/activity-type/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "id": "@id",
    "type": "@type",
    "rim": "https://w3id.org/ogc/rim/",
    "dct": "http://purl.org/dc/terms/",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "prov": "http://www.w3.org/ns/prov#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://nielshoffmann.github.io/registered-item-model/build/annotated/model/registered-item/activity-type/context.jsonld)

## Sources

* [W3C PROV-O](https://www.w3.org/TR/prov-o/)
* [P-Plan Ontology](https://www.opmw.org/model/p-plan/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/NielsHoffmann/registered-item-model](https://github.com/NielsHoffmann/registered-item-model)
* Path: `_sources/activity-type`

