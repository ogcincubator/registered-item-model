# ML Activity Types Profile

A [profile](https://ogcincubator.github.io/bblocks-docs/) of the
[Geoprocessing Activity Types Profile](../geoprocessing-activity-types), and so of the
[Activity Type Register Profile](../activity-type) and the
[Registered Item Model](../core-ontology). It describes machine learning training and inference
runs as typed register actions, built on the
[STAC Machine Learning Model (MLM) extension ontology](https://github.com/ogcincubator/bblocks-stac/tree/master/_sources/extensions/mlm-ontology).

## Why this is a profile, not a new model

MLM describes a model as a static artifact: a STAC Item or Collection with declared `mlm:tasks`,
`mlm:framework`, `mlm:accelerator`, and `mlm:input`/`mlm:output` specifications. It has no notion
of a particular *run* of that model — and the base Registered Item Model already has exactly the
class for that: `rim:RegisterAction`, a `acttype:ActivityType` recording who or what did something, when,
and why. This block adds `mlact:MLActivityType` as a subclass of `rim:RegisterAction`, because in this
ecosystem running a model is itself a governed act — typically the one that adds a derived product
to a register as a new `rim:RegisterItem`.

MLM models consume spatiotemporal assets, so `mlact:MLActivityType` is also a subclass of
`geoproc:GeoprocessingActivityType`. Its subclasses, and narrower ones a register defines (for example
"land cover segmentation"), can therefore be registered as activity types in a geoprocessing
activity type register, each with an optional plan of required steps.

## What it adds

- **Activity types** — `mlact:MLActivityType`, with `mlact:TrainingActivityType` and
  `mlact:InferenceActivityType` sub-types.
- **Constraints on the activity** — `mlact:model` (which STAC Item/Collection was run),
  `mlact:performsTask` (a sub-property of `mlm:tasks`, constrained by SHACL to the same closed MLM
  task list), and `mlact:framework`/`mlact:accelerator` (the actual runtime environment, which can
  differ from what the model declares as default).
- **Typed inputs and outputs** — `mlact:ActivityInput`/`mlact:ActivityOutput` (both `prov:Entity`):
  the actual data consumed/produced by one run, as distinct from MLM's `ModelInput`/`ModelOutput`
  (the model's declared *specification*). Each links back to the specification it realises via
  `dct:conformsTo`, and to its actual tensor shape/dtype via `mlact:structure`, reusing MLM's own
  `InputStructure`/`ResultStructure` classes rather than redefining them.

- **Activity type constraints** — a registered subclass of `mlact:MLActivityType` must declare at
  least one `acttype:usedEntityType` that is (a subclass of) `mlact:ActivityInput`. A registered
  subclass of `mlact:InferenceActivityType` must also declare an `acttype:generatedEntityType` that is
  (a subclass of) `mlact:ActivityOutput`. `mlact:ActivityInput` is a `geo:SpatialObject`, which
  also satisfies the geoprocessing profile's spatial-data requirement. `mlact:ActivityOutput` is
  not, because an output such as a scene classification is only a label vector.

Nothing here modifies the MLM ontology or the base Registered Item Model — it only adds new classes
and properties on top of both, which is what makes it a profile rather than a fork of either.
