
# Activity Type Register Profile (Model)

`ogc.rim.activity-type` *v0.1*

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

## Sources

* [W3C PROV-O](https://www.w3.org/TR/prov-o/)
* [P-Plan Ontology](https://www.opmw.org/model/p-plan/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/ogcincubator/registered-item-model](https://github.com/ogcincubator/registered-item-model)
* Path: `_sources/activity-type`

