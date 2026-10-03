# DMWF package review and source register

## Review method and limits

The review inventoried the supplied archive, inspected active statements across all 89 CFG files and read the five sheets in `Bentley 2024/Documentation/DynamcManagedWorkspace.xlsx`. It compared the package with Bentley's framework introduction, installation instructions and release notes on 3 October 2026. There are 241 files and 233 directory entries in the archive; the uncompressed file total is 13,180,856 bytes.

The review is static. It does not establish parser behaviour in every Bentley release, correctness of binary design resources, compatibility with a live ProjectWise server or success of the sample configurations. The findings below identify concrete source facts and likely operational consequences. Possible runtime defects are identified as candidates requiring reproduction, rather than reported as proven application failures.

## Findings register

| ID | Priority | Evidence | Consequence and recommended action |
|---|---|---|---|
| R01 | High | `Common_Predefined.cfg` rejects `_VERSION_8_11` and `_VERSION_8_10` | Exclude V8 from 24 rollout; do not infer support from remaining legacy comments |
| R02 | High | KB0021384 uses `Configuration2023` beside a `Configuration2024` path | Correct selection to actual installed folder before testing |
| R03 | High | Standard adapter falls back from intended name to `_DYNAMIC_WORKSET_DEFAULTNAME` | Check exact selected CFG; add a project-specific assertion if fallback is unacceptable |
| R04 | High | `FindInPath.cfg` line 480 assigns `FINDDIR_FOUNDROOT=${FINDDIR_D10}}` | Extra closing brace is a source anomaly in the depth-10 marker branch; reproduce in sandbox and seek a confirmed correction |
| R05 | Medium | `FindInPath.cfg` and `FindNameInPath.cfg` contain bare `%elif` branches | Parser-sensitive candidate; validate relevant ancestry-exhaustion branches with the intended engine |
| R06 | Medium | `Common_Predefined_EnvironmentCheck.cfg` line 72 contains a non-breaking-space byte after `1` | Inspect encoding and parser treatment in print-server branch; retain vendor baseline before controlled normalization |
| R07 | Medium | Common PWSetup generation value is `23` in a package with common generation `24` | Do not rely solely on trace/header numbers for release identity; record hashes |
| R08 | Medium | Workbook Variable Reference retains V8 entries, older names and advanced discovery value `1` | Use active 24 source as operational reference; annotate old workbook entries |
| R09 | Medium | Datasource defaults explicitly assign `_DYNAMIC_FINDWORKAREAROOT_ADV=0` | A prior enable setting can be overwritten; adapt at the point controlling the actual early module |
| R10 | Medium | `Common_Predefined.cfg` optional Configuration PWSetup include test uses `_DYNAMIC_CEWORKSPACEROOT_PWSETUP` | Location appears inconsistent with the section's Configuration wording; verify intended include hook and report upstream if confirmed |
| R11 | Medium | Defaults mention `_DYNAMIC_LOCAL_CONFIGURATION_ENABLED`, while workspace adapters use `_DYNAMIC_USE_LOCAL_ROOT` and `_DYNAMIC_LOCAL_ROOT` | Similarly named options do not form a single demonstrated path; follow active call chain |
| R12 | Medium | Standard adapter assigns `PW_MWP_COMPARISON_IGNORE_LIST = PW_MWP_COMPARISON_IGNORE_LIST;_DGNDIR;_DGNFILE` | Literal self-name may not preserve preexisting values as intended; inspect actual comparison result before changing |
| R13 | Medium | `IncludeInPath.cfg` uses `NESTMAX>2`, `>3` and increasing gates | Parameter is offset from intuitive depth; `6` permits four source positions before other stops |
| R14 | Medium | IncludeInPath visits current folder then parents | Parent `=` settings processed later may replace child values; test precedence |
| R15 | Medium | Drive module tests existing paths and defaults required/errors flags to `0` | Root-found state is insufficient proof of usable content; add cold-content and missing-resource tests |
| R16 | Medium | Group module defaults Organization content to Bentley root | Client Organization directory is not necessarily selected; check final `_USTN_ORGANIZATION` |
| R17 | Medium | Several example adapters restrict ORD and OpenBridge to `24.00`; this is sample workspace policy, not a DMWF product-generation limit | DMWF 24 supports `10.xx`, `23.xx`, `24.xx` and `25.xx`; technically `26.xx` requires an Application CSV specifying and locking the version because of the missing registry key. Adapt workspace checks to the approved dataset |
| R18 | Low | CSB templates reference generic `Resources/Bentley`; client Configuration PWSetup filename includes `Configuration2023` | Adapt values and use actual include patterns; a filename may be stale without changing its active contents |
| R19 | Low | File traces and comments retain old releases and misspellings | Teach actual variables and hashes; do not infer current behaviour from comment history |
| R20 | Medium | DWG rendition branch uses Bentley `MS_` variables in `ACADConfiguration` | Do not market or deploy it as a tested Civil 3D bootstrap |

Priorities indicate review attention, not a vendor severity classification. This pack does not patch the uploaded baseline.

## Findings that need runtime reproduction

For R04, construct a marker search whose valid ancestor is at the helper's depth-10 position and capture the expanded result and parser message. Compare against a shallow successful marker search. Preserve both traces.

For R05, create a test file whose ancestor chain reaches the boundary branch under examination. Test the exact supported application and Explorer combination. A static anomaly alone does not justify declaring the entire helper unusable.

For R06, inspect the byte-level encoding and execute the relevant print-server detection branch. Decide any normalization as a documented local patch with a before-and-after hash and regression result.

For R10, put an unmistakable diagnostic assignment in the intended Configuration PWSetup hook and show whether it loads. Compare group and Bentley-root routes and whether setup files are inside or outside the content Configuration. Do not assert a corrected path until the intended vendor design has been confirmed.

For R12, establish a preexisting ignore list in a sandbox, load the adapter and capture the final list and actual comparison behaviour. Check that necessary exclusions remain intact and that meaningful standards changes are not ignored.

## Published source register

| Source | Link | How used |
|---|---|---|
| Supplied package | `DMWF 24.0.0.0.zip` | Primary implementation baseline; inventory and CFG review |
| Embedded workbook | `Bentley 2024/Documentation/DynamcManagedWorkspace.xlsx` | Structure and variable documentation; compared with current CFGs |
| Bentley KB0020036 | https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0020036 | Framework purpose, single-CSB concept and links to setup/release material |
| Bentley KB0021384 | https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0021384 | Published minimal installation steps; naming inconsistency noted |
| Bentley KB0020038 | https://bentleysystems.service-now.com/community?id=kb_article_view&sysparm_article=KB0020038 | Published 24 release summary and historical changes |

## Published guidance compared with supplied implementation

KB0020036 describes DMWF as a CFG-based, context-sensitive managed-workspace template and describes a common Predefined CSB. The supplied package implements that structure but includes legacy explanatory material that predates removal of V8 support.

KB0021384 describes copying the framework and client content, setting a ProjectWise Folder root and String include, creating a WorkArea and adapting workspace selection. Its Configuration naming inconsistency requires correction against the supplied folder structure.

KB0020038 identifies V8 removal, IncludeInPath, OneMapping, Drive options, iTwin checks and the changed advanced-discovery default in release 24. The reviewed files contain those features. The package's embedded workbook still contains older reference content, so it should not be treated as a fully synchronized reference for every current setting.

## Additional primary source for the Autodesk concept

Autodesk documentation on [automatic AutoLISP loading](https://help.autodesk.com/cloudhelp/2025/ENU/AutoCAD-LT-Customization/files/GUID-FDB4038D-1620-4A56-8824-D37729D42520.htm) identifies the standard startup filenames. It supports the filename clarification in the integration guide; the proposed ProjectWise profile and Drive bootstrap still requires separate implementation testing.

## Distribution and attribution

This repository contains independent guidance and training material prepared by Christopher J Andrew. Bentley's template and product names are acknowledged as their respective owner's work. It is not an official Bentley publication or a Bentley certification course.

The repository does not redistribute the vendor ZIP, binary resources or complete vendor CFG files. Obtain the framework through Bentley's linked article and follow its applicable terms. Minimal configuration examples and the occurrence index explain the reviewed implementation; vendor ownership of the underlying template is unchanged.

The source manifest identifies the reviewed baseline. If a later download has different hashes, repeat the relevant review and acceptance tests rather than assuming the findings apply unchanged.

## Release naming and 26.xx operational clarification

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

The product-family support range and Application CSV requirement are operational clarifications incorporated into this edition. The archive confirms the registry-based detection route and sample-specific version checks; no 26.xx runtime test or CSV schema validation was performed in this static review.
