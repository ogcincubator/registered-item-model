# Registered Item Model

A base RDF and SHACL model for register items, register item classes and register governance
metadata, published as an [OGC Building Block](https://ogcincubator.github.io/bblocks-docs/) and
based on ISO 19135:2026 (*Geographic information — Registration and register governance*).

## Why this exists

A register is a catalogue with systematic governance: a content information model (what an item
looks like) plus management metadata (who governs it, what state it is in, how it changes).
ISO 19135:2026 defines one such register model — separating a *concept plane* (meaning) from a
*content plane* (representation), and adding roles, statuses, actions, relations and commitments
on top.

Many implementations already realise something like this model with their own, unrelated schemas
— a CSV catalogue, the OGC Definitions Server, a GitHub-managed vocabulary, a Building Blocks
register. Rather than each inventing its own governance vocabulary, this repository publishes the
ISO 19135:2026 register model once, as a single base building block
([`_sources/core-ontology`](_sources/core-ontology)). Any register maps its own schema's
identifiers, statuses and roles onto it via a JSON-LD context, so that management metadata from
otherwise unrelated registers becomes one queryable graph — and a sub-register expresses its own
rules as a **profile** (null, additive or constrained) of this base block, always validating
against it. Building Blocks itself is a register, so its own specification can in principle be
built on this same base.

See [`_sources/core-ontology/README.md`](_sources/core-ontology/README.md) for what the model
covers, and the block's `bblock.json` `sources` for the ISO 19135:2026 reference and the companion
design analysis this repository is grounded in.

## JSON / JSON-LD binding

The core block is RDF (OWL + SKOS + SHACL). The six blocks under [`_sources/rim`](_sources/rim)
— `register`, `register-specification`, `register-item-class`, `registered-item`,
`concept-version`, `register-action` — are its JSON binding: one JSON Schema and one JSON-LD
context each, mapping plain JSON keys (`objectIdentifier`, `itemClass`, `validityStatus: "valid"`,
`actionType: "addition"`, ...) onto the core terms and SKOS values. They define no term of their
own; every class, property and status value lives once, in `core-ontology`. Their schemas pin only
the ISO 19135:2026 required core and leave `type` open, so that a sub-register can
`allOf`-reference `registered-item` while typing its items with its own sub-class of
`rim:RegisterItem` — the core SHACL shapes then apply to those items through the ontology closure.
Each block carries a JSON example and the equivalent Turtle, validated against the same shapes.

## Profiles

The profiles form one chain, each a profile of the one above it:

- [`_sources/activity-type`](_sources/activity-type): registers of `prov:Activity` types. Each
  register item is a class that specialises `prov:Activity`. It declares the types of entity,
  agent and activity it relates to, and may point to a `prov:Plan` of required steps.
- [`_sources/geoprocessing-activity-types`](_sources/geoprocessing-activity-types): adds
  `geoproc:GeoprocessingActivity` and requires each registered sub-type to use or generate at least
  one GeoSPARQL spatial data type.
- [`_sources/ml-activity-types`](_sources/ml-activity-types): machine learning
  training/inference runs, built on the STAC Machine Learning Model (MLM) extension's ontology.
  `mlact:MLActivity` is both a `geoproc:GeoprocessingActivity` and a `rim:RegisterAction`.

Profile blocks share
`"group": "Registered Item Model"` in their `bblock.json` so the viewer groups them together.

## Where this could live

This repository realises the model as a standalone Building Blocks register. That was one of
several options for where a base register model could be maintained — a cross-domain semantic
model, a Building Blocks foundation model, or a general OGC registers model are the others — and
the deciding question in each case is who is accountable for changing the base model. A standalone
repository keeps that accountability narrow and the model easy to iterate on; it can migrate into
a broader home later without breaking anything that already depends on it, since the identifier
prefix is designed to remain stable across such a move.

## Repository structure

This is a standard [`bblock-template`](https://github.com/opengeospatial/bblock-template)
register. See [`bblocks-config.yaml`](bblocks-config.yaml) for the register configuration, and the
[bblocks authoring documentation](https://ogcincubator.github.io/bblocks-docs/) for how to build,
test and extend it.

```bash
./build.sh   # process and validate the building blocks (requires Docker)
./view.sh    # preview the register in the bblocks viewer (requires Docker)
```
