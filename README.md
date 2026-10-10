# Awesome Requirements AI

English | [日本語](README.ja.md)

Curated list of open-source tools for managing **hierarchical requirement trees** at the level of a **whole vehicle** or a **whole company IT system**, for **updating the tree after development is finished** (as-built sync, baselines, traceability), and for doing this work **with AI** (LLMs, agents, MCP).

## Contents

- [Requirements as Code (text + Git)](#requirements-as-code-text--git)
- [Requirements Editors / GUI](#requirements-editors--gui)
- [MBSE: Whole Vehicle / System Models](#mbse-whole-vehicle--system-models)
- [Capella x StrictDoc Connection](#capella-x-strictdoc-connection)
- [EA: Whole Company IT](#ea-whole-company-it)
- [Traceability](#traceability)
- [Commercial Tools and Their GitHub Integrations](#commercial-tools-and-their-github-integrations)
- [AI for Requirements](#ai-for-requirements)
- [Related Lists](#related-lists)
- [Suggested Combinations](#suggested-combinations)

## Requirements as Code (text + Git)

- [doorstop-dev/doorstop](https://github.com/doorstop-dev/doorstop) ![stars](https://img.shields.io/github/stars/doorstop-dev/doorstop?style=flat-square) - Requirements management using version control; parent/child links between documents form the tree.
- [strictdoc-project/strictdoc](https://github.com/strictdoc-project/strictdoc) ![stars](https://img.shields.io/github/stars/strictdoc-project/strictdoc?style=flat-square) - Technical documentation and requirements management with a web UI for editing hierarchies.
- [florath/rmtoo](https://github.com/florath/rmtoo) ![stars](https://img.shields.io/github/stars/florath/rmtoo?style=flat-square) - Free and open-source requirements management tool (Python); requirements in plain text files, dependency/traceability checks, LaTeX/PDF/HTML output.
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
- [goeb/reqflow](https://github.com/goeb/reqflow) ![stars](https://img.shields.io/github/stars/goeb/reqflow?style=flat-square) - Requirement traceability tool (C++).
- [elisa-tech/BASIL](https://github.com/elisa-tech/BASIL) ![stars](https://img.shields.io/github/stars/elisa-tech/BASIL?style=flat-square) - Software quality management with requirement traceability.

## Commercial Tools and Their GitHub Integrations

The tools below are commercial (not open source), so they are not listed as main entries. Only integration repositories found on GitHub are listed. IBM DOORS Next, ReqView and Cradle have no official repository that I could confirm.

- [valispace/ValispacePythonAPI](https://github.com/valispace/ValispacePythonAPI) ![stars](https://img.shields.io/github/stars/valispace/ValispacePythonAPI?style=flat-square) - Python API for Valispace (requirements and systems engineering).
- [valispace/ValispaceExcelPlugin](https://github.com/valispace/ValispaceExcelPlugin) ![stars](https://img.shields.io/github/stars/valispace/ValispaceExcelPlugin?style=flat-square) - Valispace Excel plugin.
- [Kio-pon/innoslate-mcp](https://github.com/Kio-pon/innoslate-mcp) ![stars](https://img.shields.io/github/stars/Kio-pon/innoslate-mcp?style=flat-square) - MCP server that lets an AI assistant read and fill an Innoslate project through your own logged-in browser (unofficial, early stage).
- [FrancoisMalan/innoslate-xml-parser](https://github.com/FrancoisMalan/innoslate-xml-parser) ![stars](https://img.shields.io/github/stars/FrancoisMalan/innoslate-xml-parser?style=flat-square) - Parses an Innoslate project's exported XML file into custom CSV files (old, 2019).

## AI for Requirements

Most entries below are young projects with few stars; check maturity before relying on them. Descriptions are taken from each repository's GitHub description.

### Agent skills and spec-driven development

- [Agile-V/agile_v_skills](https://github.com/Agile-V/agile_v_skills) ![stars](https://img.shields.io/github/stars/Agile-V/agile_v_skills?style=flat-square) - Agent Skills for traceable requirements, independent verification, human approval gates, and auditable AI-assisted engineering (Claude Code, Cursor, VS Code, GitHub Copilot).
- [sudokar/openspec-plus](https://github.com/sudokar/openspec-plus) ![stars](https://img.shields.io/github/stars/sudokar/openspec-plus?style=flat-square) - Agentic skills that extend OpenSpec spec-driven development: discovery, requirements, design decisions, execution planning.
- [FredAntB/Spec-Driven-Development](https://github.com/FredAntB/Spec-Driven-Development) ![stars](https://img.shields.io/github/stars/FredAntB/Spec-Driven-Development?style=flat-square) - Claude skill that generates requirements.md, design.md and tasks.md before code, plus config files for several AI coding tools.
- [agent-ix/quoin](https://github.com/agent-ix/quoin) ![stars](https://img.shields.io/github/stars/agent-ix/quoin?style=flat-square) - Spec-driven development for Claude Code with validated, ISO/IEC/IEEE 29148-aligned specs.
- [muchengracedriver/awesome-requirements-writer](https://github.com/muchengracedriver/awesome-requirements-writer) ![stars](https://img.shields.io/github/stars/muchengracedriver/awesome-requirements-writer?style=flat-square) - Turns engineering intent into requirements that can be designed, reviewed, verified, and traced; flags missing inputs instead of inventing thresholds.
- [kj648/requirement-clarifier](https://github.com/kj648/requirement-clarifier) ![stars](https://img.shields.io/github/stars/kj648/requirement-clarifier?style=flat-square) - Requirement-engineering workflow that turns ambiguous requirements into verified, traceable specs for AI coding agents.

### LLM-assisted analysis, traceability, and change impact

- [saima-imran/safe-trace-ai](https://github.com/saima-imran/safe-trace-ai) ![stars](https://img.shields.io/github/stars/saima-imran/safe-trace-ai?style=flat-square) - Research prototype for AI-assisted safety-requirements change-impact analysis, traceability, and verification-evidence review (Ollama, Streamlit).
- [rioffe/speccheck](https://github.com/rioffe/speccheck) ![stars](https://img.shields.io/github/stars/rioffe/speccheck?style=flat-square) - For every ID a SPEC.md declares, finds where code and tests cite it and reports one evidence-backed status per ID; optional downgrade-only LLM judge.
- [ahmedsalem84/Req-RAG-LLM](https://github.com/ahmedsalem84/Req-RAG-LLM) ![stars](https://img.shields.io/github/stars/ahmedsalem84/Req-RAG-LLM?style=flat-square) - RAG-enhanced LLMs with knowledge-graph construction and traceability optimization for requirements engineering.
- [boxdate/ReqPipeline](https://github.com/boxdate/ReqPipeline) ![stars](https://img.shields.io/github/stars/boxdate/ReqPipeline?style=flat-square) - USDM and local-LLM (Ollama) requirements engineering pipeline.

### MCP servers for requirements tools

- [XORwell/jama-cli](https://github.com/XORwell/jama-cli) ![stars](https://img.shields.io/github/stars/XORwell/jama-cli?style=flat-square) - Command-line interface for Jama requirements management with MCP server support for AI assistants.
- [arthurfantaci/requirements-advisor](https://github.com/arthurfantaci/requirements-advisor) ![stars](https://img.shields.io/github/stars/arthurfantaci/requirements-advisor?style=flat-square) - MCP server providing guidance on requirements management best practices.
- [aminfara/matrix](https://github.com/aminfara/matrix) ![stars](https://img.shields.io/github/stars/aminfara/matrix?style=flat-square) - Local, project-scoped requirement-management MCP server for multi-agent, multi-session work.
- [Kio-pon/innoslate-mcp](https://github.com/Kio-pon/innoslate-mcp) ![stars](https://img.shields.io/github/stars/Kio-pon/innoslate-mcp?style=flat-square) - MCP server for Innoslate (see the commercial tools section above).

### MBSE with LLM agents

- [cosmindxu/mbse-workflow](https://github.com/cosmindxu/mbse-workflow) ![stars](https://img.shields.io/github/stars/cosmindxu/mbse-workflow?style=flat-square) - LLM-agent workflow that authors layered, SysML-like models and gates every step on Sysprose checks.

### Requirements for AI systems themselves

- [responsibleai/ASSERT](https://github.com/responsibleai/ASSERT) ![stars](https://img.shields.io/github/stars/responsibleai/ASSERT?style=flat-square) - Requirement-driven evaluation harness for AI agents and LLM applications: generates behavior-specific test cases from requirements.
- [ybeven/4D-ARE](https://github.com/ybeven/4D-ARE) ![stars](https://img.shields.io/github/stars/ybeven/4D-ARE?style=flat-square) - Attribution-driven requirements-engineering framework for LLM agents.

### Suggested AI-assisted update loop (not a tool, a workflow idea)

1. After development, collect what changed (diff, as-built model, test results).
2. Let an LLM propose changes to the requirement tree and trace links, with change-impact analysis.
3. A human reviews and approves; the tree stays in Git (`.sdoc` / Doorstop) so every change is a reviewable diff.
4. Re-export (ReqIF) and re-check traceability.

## Related Lists

- [jgsystemsconsulting/awesome-requirements-engineering](https://github.com/jgsystemsconsulting/awesome-requirements-engineering) - Requirements engineering resources: standards, methods, interchange formats, tools, books, community.
- [ishandutta2007/Awesome-Engineering-Requirements-Management](https://github.com/ishandutta2007/Awesome-Engineering-Requirements-Management) - Open-source engineering requirements management tools.

## Suggested Combinations

| Scope | Top-level model | Requirement tree | After development |
|---|---|---|---|
| Whole vehicle | Capella or SysML v2 | StrictDoc / Doorstop | Script updates via py-capellambse / SysML v2 API; Capella x StrictDoc via ReqIF (see above) |
| Whole company IT | waltz / turbo-ea | StrictDoc | Re-sync baselines in Git, trace with OpenFastTrace |
| Either, with AI | as above | as above | LLM proposes tree changes and impact; human approves in Git (see AI section) |

## Contributing

Pull requests are welcome. Add one entry per line with a short description.
