# Awesome Requirements Tree

English | [日本語](README.ja.md)

Curated list of open-source tools for managing **hierarchical requirement trees** at the level of a **whole vehicle** or a **whole company IT system**, and for **updating the tree after development is finished** (as-built sync, baselines, traceability).

## Contents

- [Requirements as Code (text + Git)](#requirements-as-code-text--git)
- [Requirements Editors / GUI](#requirements-editors--gui)
- [MBSE: Whole Vehicle / System Models](#mbse-whole-vehicle--system-models)
- [Capella x StrictDoc Connection](#capella-x-strictdoc-connection)
- [EA: Whole Company IT](#ea-whole-company-it)
- [Traceability](#traceability)
- [Suggested Combinations](#suggested-combinations)

## Requirements as Code (text + Git)

- [doorstop-dev/doorstop](https://github.com/doorstop-dev/doorstop) ![stars](https://img.shields.io/github/stars/doorstop-dev/doorstop?style=flat-square) - Requirements management using version control; parent/child links between documents form the tree.
- [strictdoc-project/strictdoc](https://github.com/strictdoc-project/strictdoc) ![stars](https://img.shields.io/github/stars/strictdoc-project/strictdoc?style=flat-square) - Technical documentation and requirements management with a web UI for editing hierarchies.
- [useblocks/sphinx-needs](https://github.com/useblocks/sphinx-needs) ![stars](https://img.shields.io/github/stars/useblocks/sphinx-needs?style=flat-square) - Needs/requirements in Sphinx; used in ISO 26262 style projects.
- [bmw-software-engineering/trlc](https://github.com/bmw-software-engineering/trlc) ![stars](https://img.shields.io/github/stars/bmw-software-engineering/trlc?style=flat-square) - "Treat Requirements Like Code" from BMW; checks structure changes statically.

## Requirements Editors / GUI

- [sevendays/doorhole](https://github.com/sevendays/doorhole) ![stars](https://img.shields.io/github/stars/sevendays/doorhole?style=flat-square) - Graphical requirements editor for Doorstop.
- [shuart/ephemeris](https://github.com/shuart/ephemeris) ![stars](https://img.shields.io/github/stars/shuart/ephemeris?style=flat-square) - Systems engineering and requirements management app (JavaScript).
- [osrmt/osrmt](https://github.com/osrmt/osrmt) ![stars](https://img.shields.io/github/stars/osrmt/osrmt?style=flat-square) - Open Source Requirements Management Tool (Java).

## MBSE: Whole Vehicle / System Models

- [dbinfrago/py-capellambse](https://github.com/dbinfrago/py-capellambse) ![stars](https://img.shields.io/github/stars/dbinfrago/py-capellambse?style=flat-square) - Headless Python access to Capella models; script bulk updates after development.
- [Systems-Modeling/SysML-v2-Release](https://github.com/Systems-Modeling/SysML-v2-Release) ![stars](https://img.shields.io/github/stars/Systems-Modeling/SysML-v2-Release?style=flat-square) - SysML v2 specification releases.
- [Systems-Modeling/SysML-v2-API-Services](https://github.com/Systems-Modeling/SysML-v2-API-Services) ![stars](https://img.shields.io/github/stars/Systems-Modeling/SysML-v2-API-Services?style=flat-square) - REST API to read and update SysML v2 models.
- [Open-MBEE/exec-cameo-mdk](https://github.com/Open-MBEE/exec-cameo-mdk) ![stars](https://img.shields.io/github/stars/Open-MBEE/exec-cameo-mdk?style=flat-square) - Cameo plugin for model sync (MMS) and document generation.
- [gaphor/gaphor](https://github.com/gaphor/gaphor) ![stars](https://img.shields.io/github/stars/gaphor/gaphor?style=flat-square) - Simple SysML/UML modeling tool.

## Capella x StrictDoc Connection

How to connect a requirement tree (StrictDoc) with a system architecture model (Eclipse Capella).

**Pattern (as shown by the pilots below)**

```text
StrictDoc (.sdoc, canonical requirements, in Git)
   -> ReqIF 1.0 export (native StrictDoc export)
   -> Eclipse Capella + Requirements Viewpoint (manual ReqIF import / Diff-Merge)

Requirement <-> architecture links: kept outside both tools in trace-links.yaml (by MID and UUID)
Capella model read access: py-capellambse (read-only)
```

- Keep the requirement tree in `.sdoc` as the single source of truth; re-export ReqIF after every change.
- Version the exchange (e.g. `v1` / `v2` Git tags) and compare ReqIF snapshots by `SPEC-OBJECT/@IDENTIFIER` (UID) to see what changed.
- Do not write to Capella files from the requirement side; read the model through py-capellambse.

**Pilot repositories (early stage, 0 stars at time of writing)**

- [dechernov-stack/reqpilot-sc](https://github.com/dechernov-stack/reqpilot-sc) ![stars](https://img.shields.io/github/stars/dechernov-stack/reqpilot-sc?style=flat-square) - StrictDoc 0.29.0 -> ReqIF 1.0 -> Capella 7.1.0 (Requirements Viewpoint 0.14.0) interoperability pilot; includes v1/v2 ReqIF snapshots and comparison by UID.
- [dechernov-stack/reqpilot-workbench](https://github.com/dechernov-stack/reqpilot-workbench) ![stars](https://img.shields.io/github/stars/dechernov-stack/reqpilot-workbench?style=flat-square) - Local web workbench over StrictDoc and Capella: requirement CRUD, architecture tree, traceability graph, coverage matrices, impact analysis.

**Known limitations reported by the pilots**

- The real Capella import test was **not executed** in either pilot (no Capella GUI/model on the build machine); only a fixture was used.
- StrictDoc 0.29.0 native ReqIF export wrote literal `\uXXXX` sequences for Cyrillic text; non-ASCII text (e.g. Japanese) may need checking before import. Not verified for Japanese.
- StrictDoc 0.29.0 and capellambse 0.8.0 need incompatible `python-datauri` versions, so the workbench uses two separate virtual environments.

## EA: Whole Company IT

- [finos/waltz](https://github.com/finos/waltz) ![stars](https://img.shields.io/github/stars/finos/waltz?style=flat-square) - Enterprise information service: applications, data, and organization relationships.
- [vincentmakes/turbo-ea](https://github.com/vincentmakes/turbo-ea) ![stars](https://img.shields.io/github/stars/vincentmakes/turbo-ea?style=flat-square) - Enterprise architecture management solution.
- [ModelioOpenSource/Modelio](https://github.com/ModelioOpenSource/Modelio) ![stars](https://img.shields.io/github/stars/ModelioOpenSource/Modelio?style=flat-square) - Modeling for ArchiMate, TOGAF, UML, BPMN.
- [tractorjuice/arc-kit](https://github.com/tractorjuice/arc-kit) ![stars](https://img.shields.io/github/stars/tractorjuice/arc-kit?style=flat-square) - Architecture governance with requirements and traceability using AI coding assistants.

## Traceability

- [itsallcode/openfasttrace](https://github.com/itsallcode/openfasttrace) ![stars](https://img.shields.io/github/stars/itsallcode/openfasttrace?style=flat-square) - Requirement tracing suite.
- [elisa-tech/BASIL](https://github.com/elisa-tech/BASIL) ![stars](https://img.shields.io/github/stars/elisa-tech/BASIL?style=flat-square) - Software quality management with requirement traceability.

## Suggested Combinations

| Scope | Top-level model | Requirement tree | After development |
|---|---|---|---|
| Whole vehicle | Capella or SysML v2 | StrictDoc / Doorstop | Script updates via py-capellambse / SysML v2 API; Capella x StrictDoc via ReqIF (see above) |
| Whole company IT | waltz / turbo-ea | StrictDoc | Re-sync baselines in Git, trace with OpenFastTrace |

## Contributing

Pull requests are welcome. Add one entry per line with a short description.
