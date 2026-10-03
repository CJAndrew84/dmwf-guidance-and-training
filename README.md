# DMWF 24 guidance and training

I have prepared this reference pack to explain how the Dynamic Managed Workspace Framework connects ProjectWise project context to Bentley application standards. It brings the framework, workspace adaptation, project selection, deployment and support into one consistent operating model.

**Author:** Christopher J Andrew  
**Guidance edition:** 1.0, 3 October 2026  
**Reviewed baseline:** supplied DMWF 24.0.0.0 package  
**Status:** source-reviewed guidance; live ProjectWise and application acceptance tests remain required.

## Start here

| Material | Intended use |
|---|---|
| [Deployment and configuration handbook](01-Handbook.md) | Understand architecture, load order, selection and optional features |
| [Administrator runbook](02-Administrator-Runbook.md) | Install, onboard clients, test, deploy, support and roll back |
| [Configuration reference](03-Configuration-Reference.md) | Look up key variables, defaults, operators and diagnostics |
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
