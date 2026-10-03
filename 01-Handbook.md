# DMWF 24 deployment and configuration handbook

Prepared by Christopher J Andrew • Guidance edition 1.1 • 3 October 2026

## Purpose and use

We use the Dynamic Managed Workspace Framework to connect ProjectWise project context to a repeatable Bentley application configuration. This handbook explains how we select standards, resolve WorkSets and manage deployment without building a different framework for every project. Read it alongside the administrator runbook, configuration reference and training workbook in this pack.

The review covers the supplied **DMWF 24.0.0.0.zip**, its 89 configuration files and its embedded documentation workbook, plus Bentley knowledge articles KB0020036, KB0021384 and KB0020038. It is a static review of the delivered configuration and published guidance. No ProjectWise datasource, Windows registry, Bentley application, rendition server or iTwin processing service was available for execution testing. Examples therefore require acceptance testing in the intended environment before production use. Binary DGN, DGNLIB and DGNWS contents were inventoried, not functionally examined in a Bentley application.

**Our implementation principle:** keep the common framework stable, express project selection in WorkArea setup files and adapt each workspace through its own PWSetup layer. Test the resolved configuration and drawing output before releasing it. A file opening successfully is necessary, but does not prove that the intended standards loaded.

The labels used throughout this pack are:

| Label | Meaning |
|---|---|
| Verified in package | Directly visible in the supplied CFG files or workbook |
| Published guidance | Stated in the Bentley articles identified in the source register |
| Recommendation | An operating practice proposed here, rather than a delivered DMWF feature |
| Requires runtime verification | Depends on ProjectWise integration, registry, application version or binary content |

## What DMWF supplies

DMWF supplies configuration routing and examples for a ProjectWise managed workspace. It determines roots and names using the opened document's context, reads selection information and redirects Bentley framework variables. The delivered files also contain validation messages, product version checks, rendition and iTwin environment detection, optional ProjectWise Drive redirection and directory search helpers.

DMWF is not, by itself, a standards authoring system or a complete release management service. It does not demonstrate that a client dataset complies with a PDF standard, that an add-in is approved, that every datasource contains identical content or that an individual workstation has hydrated its ProjectWise Drive files. Those are separate responsibilities that we manage around the framework.

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

The common file rejects V8 family configurations. Some supplied workspace examples explicitly restrict OpenRoads Designer and OpenBridge Modeler to `24.00`. These are sample workspace policy checks, not a framework-wide restriction to 2024 products. Adapt those checks to the approved dataset and application combination. Folder names such as `Bentley 2024` and `Configuration2024` describe the supplied package layout; they do not define the framework support range.

## The five different things we must keep separate

| Concept | Purpose | Typical example |
|---|---|---|
| Common framework | Resolves context and redirects configuration | `Bentley 2024/Common_Predefined.cfg` |
| Configuration | Contains an Organization folder and a Workspaces collection | `ClientWorkspaces/ClientName/Configuration2024/` |
| Workspace | Standards shared by multiple projects | `CEWorkspaceExampleAdv` |
| ProjectWise WorkArea | Project container that gives documents managed project context | A test project converted to a WorkArea |
| Bentley WorkSet | Project or package configuration and working roots used by the application | A selected `1234.cfg` and corresponding project root |

A WorkArea and a WorkSet can have the same name and root, but they need not. A single WorkArea can contain several contract packages, each with a different WorkSet. This is why `_USTN_WORKSETCFG`, `_USTN_WORKSETROOT` and `_USTN_WORKSETNAME` must be checked independently. The CFG can live in a protected setup folder while design files and project standards live elsewhere.

A connected project is another distinct context. The common configuration obtains connected project information separately from `DMS_PROJECT` WorkArea information. Do not assume that an iTwin connection establishes the correct WorkSet or that the nearest nested WorkArea is always the desired project root.

## Package layout and responsibility

The ZIP has two principal branches. `Bentley 2024` contains common files, helper files, PWSetup modules, a built-in example Configuration, DWG rendition resources and documentation. `ClientWorkspaces/ClientName` contains the client-group example, a `Configuration2024` branch, the advanced workspace and several project selection examples.

| Location relative to package root | Responsibility | Editing rule |
|---|---|---|
| `Bentley 2024/Common_Predefined.cfg` | Main Predefined framework | Preserve baseline; vendor file says not to edit |
| `Bentley 2024/_PWSetup/Common_Predefined_PWSetup.cfg` | Coordinates common setup modules | Controlled adaptation point |
| `Bentley 2024/_PWSetup/Common_Predefined_DatasourceDefaultSettings.cfg` | Default names and switches | Set datasource defaults deliberately |
| `Bentley 2024/_PWSetup/OneMapping/` | Product group and subgroup mappings | Validate selected product mapping |
| `Bentley 2024/FindNameInPath.cfg` | Ancestor name and nested folder selection | Treat as helper implementation |
| `Bentley 2024/FindInPath.cfg` | Name or marker based path discovery | Treat as helper implementation |
| `Bentley 2024/IncludeInPath.cfg` | Includes matching CFG files while walking ancestors | Protect include locations and test boundaries |
| `ClientWorkspaces/ClientName/_PWSetup/` | Client-group setup | Own client adaptation here |
| `.../Configuration2024/Workspaces/<Workspace>/_PWSetup/` | Workspace root and WorkSet adaptation | Use one explicitly selected setup pattern |
| `<WorkArea>/_PWSetup/` | Project selection and project CFG files | Project administration under change control |
| `Bentley 2024/Documentation/ConfigurationTemplates/` | Optional additional level templates | They are examples, not automatically active files |

Keep application standards such as cells, levels, fonts and print styles in the intended native standards layer. PWSetup is principally about making that layer work in ProjectWise. Do not distribute ordinary `MS_` resource settings across unrelated Predefined setup files simply because the application happens to accept them there.

## How the delivered load sequence works

The sequence below follows active statements in `Common_Predefined.cfg` and its common setup coordinator. It describes routing during the DMWF Predefined include, not every step in the application's complete startup sequence.

1. ProjectWise starts the managed configuration through an assigned Predefined CSB. The root variable points at the installed Bentley framework folder; an include reads `Common_Predefined.cfg`.
2. The common file records its version, rejects V8 configurations and establishes datasource, connected project, WorkArea and parent WorkArea context where available.
3. It includes `Common_Predefined_PWSetup.cfg` from the framework's PWSetup folder.
4. That coordinator runs environment, ProjectWise Explorer version, product version and datasource default modules when their files exist. It then resolves the WorkArea root using the default or advanced route.
5. It optionally performs OneMapping, reads the WorkArea's Predefined setup files, performs Drive processing if enabled and resolves workspace groups.
6. Control returns to the common framework. The CONNECT branch derives the Configuration, Organization and workspace roots, checks required locations and includes the selected workspace setup pattern.
7. The framework checks the named workspace CFG and establishes and locks key `_USTN_` roots and names. Workspace setup can already have chosen the WorkSet CFG and roots before these later defaults are considered.
8. It appends validation information, optionally stops for common debug output, then reads the postprocessing hook if present.

The location and timing of an override are part of its meaning. A setting made in a WorkArea file is too late to affect a module that already ran earlier in common setup. A setting made after a `%lock` may not change the value. A `:` assignment may keep an existing value even when a later line seems to offer a new default.

See Diagram 01 for the branching load process and Diagram 02 for the selection decision.

## Bootstrap configuration

Create the root setting using ProjectWise Administrator's **ProjectWise Folder** value type and browse to the actual installed framework folder. The following is a conceptual representation; it is not a requirement to type a literal datasource string into the CSB:

```cfg
# Predefined CSB representation
_DYNAMIC_DATASOURCE_BENTLEYROOT = <ProjectWise Folder value for Bentley 2024>
%include $(_DYNAMIC_DATASOURCE_BENTLEYROOT)Common_Predefined.cfg
```

The include is a String entry in the CSB. Assign the block at the appropriate scope and confirm its inheritance for the test document. A correctly created CSB that is not assigned to the document's effective configuration will have no effect.

The root and include must resolve as a directory and filename combination. Use the directory representation supplied by ProjectWise; retain its separator. A manually composed path missing the final separator can produce a combined path that does not name a real file.

The delivered CSB example references a generic `Resources/Bentley` location rather than the actual `Bentley 2024` package folder. Treat this as an example to adapt. Do not change the package layout to match stale example text without a design reason.

## Selecting a client workspace

The normal client-group selection is recorded in a project WorkArea setup file:

```cfg
# Illustrative WorkArea selection for the supplied client example
_DYNAMIC_WORKSPACEGROUPNAME = ClientName
_DYNAMIC_WORKSPACEGROUP_CONFIGURATIONNAME = Configuration2024
_DYNAMIC_CEWORKSPACENAME = CEWorkspaceExampleAdv
_DYNAMIC_WORKSPACEPWSETUP_PREDEFINED_NAME = WorkspacePWSetup_Predefined_XXXX_CE.cfg
```

The first three names correspond to `ClientWorkspaces/ClientName/Configuration2024/Workspaces/CEWorkspaceExampleAdv/`. The last setting chooses a specific adapter filename. Exact selection is a recommendation for production predictability; the framework also accepts its delivered wildcard naming patterns.

For the built-in example under `Bentley 2024/Configuration`, the WorkArea file can leave the client-group name unset and select `CEWorkspaceExample`. Common setup then activates its Bentley-root group route. Do not set the literal client name just because the deployment folder contains a sibling `ClientWorkspaces` tree.

Workspace group discovery checks the sibling `ClientWorkspaces` location first and then `WorkspaceGroups`, based on the configured names. If both contain a matching group, the first branch wins. This is not an automatic merge of two standards libraries. A deliberately specified full group root is preferable when deployment layout differs from the examples.

## Configuration operators and load order

| Syntax | Working meaning | Why it matters |
|---|---|---|
| `NAME = value` | Assign a value | Can replace an earlier value when not prevented by locking or parser rules |
| `NAME : value` | Set a default if not already defined | A prior setting can keep a later default from taking effect |
| `NAME > value` | Append to the variable's existing value | Used for resource paths and DMWF trace messages |
| `NAME < value` | Prepend to the variable's existing value | Can change search order or message ordering |
| `%include path` | Process another CFG at this point | The included statements are part of the current sequence |
| `%if`, `%elif`, `%else`, `%endif` | Conditional processing | Branches depend on current values and context |
| `%lock NAME` | Protect a configuration value from subsequent changes | Make the final intended selection before the lock |
| `%undef NAME` | Remove a definition | Used by helper cleanup and can remove debug evidence |
| `$(NAME)` and `${NAME}` | Variable expansion forms used in the package | Preserve delivered use, especially when copying helper outputs before cleanup |
| `%error message` | Raise a configuration error and interrupt normal loading | Debug switches using this can intentionally stop startup |

This is a practical reference for the syntax present in the supplied files, not a complete Bentley configuration language specification. Test expression evaluation and path expansion in the intended product. Do not treat CFG syntax as PowerShell, shell syntax or AutoLISP.

For example, a project selection made with `=` before a workspace's `:` default should be retained. A later `:` default does not provide an override. Conversely, a default module that uses `=` can overwrite an earlier proposed value. The supplied default file uses `_DYNAMIC_FINDWORKAREAROOT_ADV=0`; enabling advanced discovery in a CSB before the common include therefore requires examining that assignment, not merely adding another early default.

## Choosing the WorkSet pattern

### One WorkArea with one WorkSet

Use the standard `WorkspacePWSetup_Predefined_XXXX_CE.cfg` adapter as the starting point. It looks for `_PWSetup/WorkSets/` under the resolved WorkArea. If that location is not present, it falls back to the workspace's `WorkSets/` collection. It tries the selected project or WorkSet name, then the configured default name, and raises an error if neither CFG exists.

This fallback is convenient for demonstrations. For a production project, a successful fallback can conceal a missing project configuration. Record the expected WorkSet CFG and make its match an acceptance criterion. If project-specific selection is mandatory, add a controlled check in the adapter rather than accepting an unrelated example default.

The design-file root is resolved separately from the CFG collection. `_DYNAMIC_WORKAREA_SUBPATH` can point below the WorkArea, while `_USTN_WORKSETSDGNWSROOT` defaults to the selected WorkSets CFG collection in the standard example. Treat DGNWS placement as a separate decision with suitable permissions.

### Several packages at a fixed folder depth

Use the FindByNest example when the folder hierarchy is stable. It searches upward for the WorkArea name, then selects a child at the configured nest depth. The delivered template uses `FINDDIR_NEST=2` and copies the discovered name to `_DYNAMIC_WORKSET_NAME`.

For a design file under `<WorkArea>/CADFiles/1234/DGN/`, a nest depth of two below the found WorkArea is intended to select `1234`. Move the file to a hierarchy without `CADFiles`, or insert another container folder, and the same depth can select a different folder. Test every approved hierarchy rather than assuming the numeric setting means a universal package identifier.

The helper walks ancestors, not the whole datasource. Its source contains an explicitly unrolled search through twelve directory positions. This is different from the WorkArea discovery module's maximum of four nested WorkArea contexts. See Diagram 03.

### Packages identified by markers

Use the FindByExists example when structure varies but a unique marker identifies the desired context. The supplied adapter uses `_PWSetup/` to locate a root and `DGN/` as its nested marker. Those are example markers; broad folder names can be ambiguous in real projects.

Define a marker contract before deployment: which folder carries it, who creates it, which file or subfolder proves its identity, and what happens if two candidates match. A marker should distinguish a valid package from an incidental folder. Always inspect `_DYNAMIC_WORKSET_NAME`, `_DYNAMIC_WORKAREA_SUBPATH` and the final CFG after a marker search.

### Additional context along the path

The Nest Plus Include example adds `IncludeInPath.cfg` before selecting the FindByNest adapter. It sets a matching pattern for `PWSetupByFind_Predefined*.cfg`, continues upward after a match, and stops at the WorkArea's name or its implemented search boundary.

The helper processes the current directory first and then its ancestors. This is important if several matching files assign the same variable: a parent assignment processed later can replace a lower folder's value. It does not establish conventional parent-first inheritance. The actual result depends on each assignment operator, existing definitions and locks. See Diagram 04.

Keep only approved configuration files in these matching locations. A folder-level setup file is executable configuration input; it should not be editable by everyone who can save design drawings.

## ProjectWise Drive and local content

The common coordinator processes Drive after the WorkArea selector and before workspace groups and final framework root calculations. Drive support is disabled by default. When enabled, the delivered module looks for the organization root through a current-user registry entry, then a user-profile path. It records whether the root was found and may redirect a workspace group or Configuration root to local Drive paths.

Set `_PROJECTWISE_DRIVE_ORG` to the actual configured organization name. `Bentley Systems Inc` in the default file is a sample. Set the enabled and required flags early enough for the Drive module. A flag assigned only in the later workspace adapter cannot reliably influence processing that has already occurred.

The module checks path existence. It does not demonstrate that the sync contains the intended release, that placeholders have been hydrated or that access to every required file succeeds. Test a newly enrolled user, an empty cache, a hydrated cache and a workstation on which Drive is unavailable. If local content is mandatory, a missing root should produce an understandable failure rather than a quiet change of standards source.

Drive redirection is also distinct from the local-root helper. The latter is included by selected workspace adapters when `_DYNAMIC_USE_LOCAL_ROOT` is enabled and its root exists. The package contains inconsistent local-option names between the default module and adapter. Use the verified active call chain; do not assume every similarly named flag activates the feature. See the review register.

## Product mapping and compatibility

The OneMapping module fills `_USTN_PRODUCT_ONE_GROUPNAME` and subgroup information when the relevant values are not already defined. The supplied OpenRoads mapping assigns `Civil` and `Road`; OpenRail assigns `Civil` and `Rail`. Common setup automatically enables its mapping path for detected ProjectWise Explorer major value `10`, while also allowing an explicit enable switch.

A mapping determines configuration structure. It does not install a product, select its executable or certify a dataset. Check `_ENGINENAME`, the detected product version and actual standards location together. A renamed executable, different display name or absent registry entry can change detection results.

When product version detection cannot obtain a usable value in the implemented failure branches, the package uses `00.00.00.00` and a not-found flag. This is an unknown version signal, not a compatible release. Decide whether the project blocks unknown versions and test the outcome. The example restrictions compare generation and major values; they do not necessarily enforce an exact build.

### Product generations and the 26.xx configuration requirement

| Product version family | Framework position | Deployment requirement |
|---|---|---|
| `10.xx`, `23.xx`, `24.xx`, `25.xx` | Supported by DMWF 24 | Select the intended product and validate the workspace dataset |
| `26.xx` | Technically supported; existing registry detection is affected by a missing key | Use an Application CSV to specify and lock the actual product version |

For 26.xx, configure the Application CSV for the applicable application, specify its actual installed product version and lock that value before DMWF product-version processing. Verify the effective full version and generation/major values in the configuration trace, then test the intended workspace and output. Keep the CSV with the controlled deployment configuration and update its locked version when the application changes. Do not invent a registry entry or replace the unknown-version value with a guessed version. Exact CSV field names and import syntax must follow the application configuration tooling in use; no unverified CSV schema is supplied here.

## Desktop and server processing

The environment module contains separate checks for workstation context, iTwin sync engine, rendition process indicators and DWG files. These create configuration flags that adapters can use. Detection is not validation of the server's dataset or permissions.

The supplied adapters include rendition-specific settings such as disabling cursor prompts and turning off the ordinary product check for a detected rendition job. Test that exception explicitly. A server-only exception should not accidentally make desktop applications bypass the project compatibility rule.

The `ACADConfiguration` tree is a Bentley configuration for DWG rendition resources, with settings such as `MS_DWGFONTPATH` and `MS_DGNLIBLIST`. It is not an AutoCAD or Civil 3D support deployment framework, and it does not configure Civil 3D drawing settings or distribute Autodesk plugins. An Autodesk profile plus Drive-based bootstrap can use the same governance model, but requires its own implementation and tests. See the standards and integration guide.

## Our recommended operating model

Use one released common framework baseline and separately versioned client adapters and standards datasets. Give each project a small, readable selector and an explicit compatibility record. Keep development, acceptance testing and production content apart; the acceptance environment should reproduce production structure and permissions.

We should be able to explain any startup with a chain of evidence: document context, effective CSB, WorkArea setup, selected adapter, final roots, loaded standards and drawing output. That is more useful than a screenshot showing that the application launched.

Do not assume DMWF automatically overlays corporate standards, client standards, discipline standards and project exceptions in that order. Establish that order through deliberate native configuration includes and resource search paths. An organization library can add corporate tools while a client library controls deliverable standards, but overlapping names need an explicit owner and precedence test.

For enterprise rollout, keep a per-datasource record of framework release, adapter release, installed root, CSB assignment, permissions, supported builds, acceptance result and rollback target. Deploy in small waves and compare resolved results across datasources. See the runbook for the release and recovery procedure.
