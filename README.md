# DMWF 24 guidance and training

I have prepared this reference pack to explain how the Dynamic Managed Workspace Framework connects ProjectWise project context to Bentley application standards. It brings the framework, workspace adaptation, project selection, deployment and support into one consistent operating model.

**Author:** Christopher J Andrew  
**Guidance edition:** 1.1, 3 October 2026  
**Reviewed baseline:** supplied DMWF 24.0.0.0 package  
**Status:** source-reviewed guidance; live ProjectWise and application acceptance tests remain required.

## Framework release and application support

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSB is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

Sample `24.00` workspace checks and `Bentley 2024` folder names do not limit the framework to 2024 products. See the handbook for the support table and 26.xx deployment procedure.

## Start here

| Material | Intended use |
|---|---|
| [Deployment and configuration handbook](01-Handbook.md) | Understand architecture, load order, selection and optional features |
| [Administrator runbook](02-Administrator-Runbook.md) | Install, onboard clients, test, deploy, support and roll back |
| [Configuration reference](03-Configuration-Reference.md) | Look up key variables, defaults, operators and diagnostics |
| [Interactive training tool](DMWF-Interactive-Training.html) | Complete eight interactive modules, simulations, lab notes and a scored assessment offline |
| [Training workbook](04-Training-Workbook.md) | Deliver a course with seven labs, model answers and competency assessment |
| [Standards and integration guidance](05-Standards-and-Integration.md) | Build datasets from standards and plan separate Autodesk integration |
| [Review and source register](06-Review-and-Source-Register.md) | Review source anomalies, evidence and verification limits |
| [Quick reference and templates](07-Quick-Reference-and-Templates.md) | Use project, release, exception and support records |
| [Workflow diagrams](08-Workflow-Diagrams.md) | View eight Mermaid workflows and their SVG companions |
| [Variable occurrence index](09-Variable-Occurrence-Index.md) | Find 258 variables across 1,448 source assignment locations |
| [Offline reading edition](DMWF-24-Reading-Edition.html) | Download and open locally to read the core guidance with diagrams |
| [Reviewed source manifest](source-manifest.json) | Compare your supplied baseline with the files reviewed |

GitHub renders the Mermaid code blocks in the diagram guide. The `.mmd` files in `diagrams/` remain editable; SVG files support viewers that do not render Mermaid. The offline HTML includes rendered SVG diagrams and requires no external JavaScript or network connection. View the HTML locally; GitHub's normal file view displays its source rather than hosting it as a website.

## Recommended reading paths

- **Workspace maintainer:** handbook, configuration reference, standards guidance and review register.
- **Datasource administrator:** handbook and administrator runbook, then the release templates.
- **Support team:** diagnostics in the runbook and configuration reference, plus the incident evidence fields.
- **Trainer:** training workbook and diagram guide, supported by a tested sandbox installation.
- **Designer:** designer verification exercise and the project selection explanation in the handbook.

## What the review found

The common Predefined routing structure is clear and reusable. The supplied package also retains older documentation, sample-specific values and some source anomalies. The review register records these without claiming unexecuted fixes or product certification. Validate exact workspace and WorkSet CFG paths; a successful launch can conceal fallback selection.

This pack distinguishes verified package behaviour, published guidance, recommended operating practices and matters requiring runtime verification. The Civil 3D material is a separate implementation concept, not a feature attributed to DMWF.

## Obtain the Bentley framework

Use [Bentley KB0020036](https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0020036) for the framework and download links. Related sources are the [installation instructions](https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0021384) and [release notes](https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0020038).

This repository does not redistribute the vendor ZIP, binary datasets or complete vendor CFGs. The source manifest records the baseline reviewed. This is an independent guidance pack, not an official Bentley publication.

## Maintaining the material

When reporting a correction, identify the guide section, source filename and line, package checksum, product and Explorer builds, expected result and actual result. Separate static observations from reproduced runtime defects. Update the guidance and source register together when a finding is resolved.

No workflows or automatic deployments are included in this documentation repository.

## Interactive training

Download `DMWF-Interactive-Training.html` and open it in a modern browser. It is one self-contained file with no external libraries or network requirement. GitHub displays HTML source; download the raw file to run it. Progress and lab notes are saved in browser storage where available. Use **Export learning record** to retain or share the results. Changing browser or file location may change the storage context. The simulations teach selected concepts; they do not execute the Bentley CFG parser or replace live acceptance tests.
