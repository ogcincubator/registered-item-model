# Registered Item Model

A base RDF and SHACL model for register items, register item classes and register
governance metadata, aligned with ISO 19135:2026 (*Geographic information — Registration
and register governance*). Sub-registers profile this model rather than inventing their own.


A register is a catalogue with systematic governance: a content information model
(what an item looks like) plus management metadata (who governs it, what state it is
in, how it changes). ISO 19135:2026 defines one such register model — separating a
**concept plane** (meaning: concepts and concept versions) from a **content plane**
(representation: register item classes and register items) and adding a governance
layer of roles, statuses, actions and commitments.

This repository publishes that model as a single base [OGC Building
Block](https://ogcincubator.github.io/bblocks-docs/), independent of any one register's
own content schema. Any register — a CSV catalogue, the OGC Definitions Server, a
GitHub-managed vocabulary, or a Building Blocks register itself — maps its own
schema's identifiers, statuses and roles onto this base model via a JSON-LD context,
so that management metadata from otherwise unrelated registers becomes one queryable
graph. A sub-register's rules are expressed as a profile of this base block (null,
additive, or constrained), always validating against it.

Register vs. registry: the **register** is the governed information (items,
identifiers, statuses, roles, processes); the **register system** — often loosely
called the "registry" — is merely the information system it happens to run on. This
model describes the former, independent of the latter.


## Building Blocks

### `ogc.model.registered-item.core-ontology` — Registered Item Model

**Type:** model

A base RDF and SHACL model for register items, register item classes and register governance metadata, based on ISO 19135:2026.

### `ogc.model.registered-item.rim.concept-version` — Concept Version

**Type:** schema

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

### `ogc.model.registered-item.rim.register` — Register

**Type:** schema

A REGISTER description model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

### `ogc.model.registered-item.rim.register-item-class` — Register Item Class

**Type:** schema

A REGISTER ITEM CLASS model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. (describes the type of object in a register) Modular Building Block to support interoperable profiles of register

### `ogc.model.registered-item.rim.register-specification` — Register Specification

**Type:** schema

Modular RDF implementation profile based on ISO/FDIS 19135:2026.

### `ogc.model.registered-item.activity-type` — Activity Type Register Profile

**Type:** model

A profile of the Registered Item Model for governed activity type records in the acttype:ActivityType hierarchy, with related PROV object types and an optional plan.

### `ogc.model.registered-item.rim.registered-item` — Registered Item

**Type:** schema

A REGISTER ITEM model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. This provides a canonical schema for a register entry, that can be extended according the REGISTER ITEM CLASS defined for the register.

### `ogc.model.registered-item.geoprocessing-activity-types` — Geoprocessing Activity Types Profile

**Type:** model

A profile of the Activity Type Register Profile for geoprocessing activity types: registered sub-classes of geoproc:GeoprocessingActivityType, each required to use or generate at least one GeoSPARQL spatial data type.

### `ogc.model.registered-item.rim.register-action` — Register Action

**Type:** schema

A REGISTER interaction activity model with JSON/LD/RDF implementation as defined by ISO/FDIS 19135:2026. Modular Building Block to support interoperable profiles of register

### `ogc.model.registered-item.st-ml-activity-types` — SpatioTemporal ML Activity Types Profile

**Type:** model

A profile of the SpatioTemporal Machine learning Activity Types Profile describing spatiotemporal machine learning training and inference runs, and registrable ML activity types, using the STAC MLM extension's task, framework/accelerator and input/output vocabulary.

