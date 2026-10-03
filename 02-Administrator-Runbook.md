# DMWF administrator deployment and support runbook

## Application version preparation

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

Before testing a 26.xx application, configure its Application CSV with the actual installed product version and lock it before DMWF version processing. Check the effective version and derived generation/major values in the trace. Confirm workspace policy checks allow the approved combination; the sample `24.00` restriction is a dataset policy example. Record the CSV, installed build, locked version and test evidence in the release record. Recheck the CSV whenever the product is upgraded.

## Scope and prerequisites

This runbook takes the supplied DMWF 24 template from an isolated test installation to a controlled production release. The ProjectWise administration steps are procedural guidance; menu names and privileges must be checked against the deployed ProjectWise build. We have not executed these steps on a live datasource.

Before starting, identify a datasource administrator, workspace maintainer, product SME and project representative. Confirm the intended desktop product and exact build, ProjectWise Explorer build, integration components and any server processing requirement. Obtain a representative design file and an agreed expected drawing output. Decide whether ProjectWise Drive is required, optional or outside scope.

Record the following deployment values before editing files:

| Field | Required decision |
|---|---|
| Framework root | Installed ProjectWise folder containing `Common_Predefined.cfg` |
| Standards root | Client group and Configuration location |
| Project root | WorkArea used by the test document |
| Selection method | Standard, nest, marker or nest plus include |
| Workspace | Exact folder name and matching CFG name |
| WorkSet | Expected name, CFG path, design root and DGNWS location |
| Compatibility | Approved product builds and server exceptions |
| Drive | Organization name, local root, sync names and missing-content policy |
| Release owner | Person who approves production deployment |
| Rollback target | Known previous framework, adapters and standards set |

## Procedure A install a minimal test environment

1. Retain the uploaded ZIP unchanged. Calculate and record its checksum and identify this as the vendor baseline. Do not rename every vendor file as part of the first installation.
2. In an isolated resource location, import the complete `Bentley 2024` and sibling `ClientWorkspaces` branches. Preserve their relative arrangement because group discovery uses the parent of the Bentley root.
3. Check that the common CFG, `_PWSetup` modules, `Configuration` folder and the client `Configuration2024` workspace exist in ProjectWise. Imported folders are not a substitute for imported documents; verify actual CFG documents are present and readable.
4. Create a Predefined CSB named clearly for the environment. Add `_DYNAMIC_DATASOURCE_BENTLEYROOT` using a ProjectWise Folder value selected by browsing. Add the common include as a String entry.
5. Assign the CSB to the agreed test scope. Review effective assignments and inherited blocks. Record any other CSB that changes workspace roots or names.
6. Copy the client's standard `CONNECTExample` project example to an isolated test-project location and make that project folder a WorkArea. Keep its `_PWSetup` structure and project CFG files.
7. Open its WorkArea selector. Verify `ClientName`, `Configuration2024` and `CEWorkspaceExampleAdv` against the installed resource path. Select the standard workspace adapter explicitly for the first test.
8. Check read permissions on the common root, client standards and setup documents using the intended ordinary test account. Confirm project and DGNWS write permissions only where needed.
9. Launch the representative design document from ProjectWise Explorer using the approved product association. Capture the final variable values and loaded CFG trace.
10. Test a cell, a level or feature definition, seed selection, font and plotted output relevant to the project. Record actual source paths, not only resource display names.

**Pass condition:** all selected names and roots match the deployment record, the expected standards load, and the representative output passes the product SME's checks. A launch using `CONNECTExample` as an unexpected fallback is a failed project-specific selection test.

**Installation article correction:** Bentley KB0021384 prints `Configuration2023` in one variable example while identifying the resource path as `Configuration2024`. For this supplied package, use the actual `Configuration2024` folder name. The archive's branch is `ClientWorkspaces`, plural; do not rely on singular wording in that page.

## Procedure B onboard a real client dataset

1. Inventory the source dataset and preserve an unchanged copy. Record its supplier, revision, licence constraints and supported product builds.
2. Launch a representative file against the original intended dataset, where feasible, and capture the expected roots and output. This provides a comparison baseline.
3. Identify the dataset's Configuration, workspace CFG and WorkSet assumptions. Search for absolute local paths, drive letters, environment variables, registry dependencies, executable startup actions and user-writable resource locations.
4. Place the dataset in a client-group branch with a stable name. Preserve its internal structure wherever possible rather than rearranging resource files to resemble the example dataset.
5. Create its Workspace PWSetup adapter. Redirect the framework roots and select the project CFG collection. Put ProjectWise-specific path adaptation here.
6. Add any environment-variable substitutes required by the client configuration before their first use. Use workspace-scoped CFG settings where appropriate so one agency's assumptions do not become global workstation settings.
7. Decide whether Organization standards come from the Bentley common root or the client's Configuration. The group module defaults `_DYNAMIC_ORGANIZATIONROOT_AT_BENTLEYROOT` to `1`; verify the final `_USTN_ORGANIZATION` rather than assuming the client Organization folder will load.
8. Create a project selector using exact group, Configuration and workspace names. Verify its adapter selection does not match several competing CFG files.
9. Compare original and managed output. Cover product-specific content such as ORD feature definitions, annotation, civil cells, templates, terrain and corridor outputs as applicable.
10. Test the second client workspace in the same environment. Confirm selection does not carry state or resource paths from the first project into the second. Only then approve the multi-client configuration.

**Pass condition:** the client's intended standards and outputs are preserved, deliberate adaptations are documented, and cross-client tests do not expose unintended resources.

## Procedure C implement a multi-package project

Choose either a fixed-depth contract or a marker contract. Record it as part of the project folder standard. Do not give every project owner a different nest depth without a reason.

For FindByNest, start with the supplied matching WorkArea and workspace templates. Verify that the selected folder at the configured depth names an existing project CFG. For the advanced client examples, the project CFG collection contains `1234.cfg` and `5678.cfg` in the relevant test structures. Copying only the main project selector without these companion files is incomplete.

For FindByExists, set a sufficiently distinctive root and package marker. Include a negative test with a missing marker and another with a duplicate marker in an unexpected ancestor. Confirm that a failed discovery does not silently open with a default standards set.

For Plus Include, inventory every matching `PWSetupByFind_Predefined*.cfg` in the ancestor chain. Record their processing order and final values. Test both branches of the project, because a local include can affect only a subset of documents.

**Pass condition:** each representative document selects its expected package name, CFG and design root. Moving a document outside the approved structure either produces the designed error or a documented alternative selection. See Diagrams 03 and 04.

## Procedure D configure optional Drive content

Enable Drive only after the managed configuration passes without it, unless the workspace depends on local-only resources. Confirm the actual Drive organization name and registry root for the test user. Configure sync names to match the variables used by the module.

Test these cases separately:

| Case | Evidence to capture | Required result |
|---|---|---|
| Drive available and hydrated | Root, selected local paths and resource hashes | Approved local content is used |
| Root exists with missing resources | Missing filename and application response | Clear failure or explicitly approved fallback |
| Drive not installed or signed in | Found flag and message | Required mode fails; optional mode follows documented policy |
| Different user's profile | Registry root and organization name | No dependency on the maintainer's personal path |
| Sync folder renamed | Selected local group or Configuration | Misconfiguration is detected |
| New release downloaded | Release identity and resource resolution | Framework and dataset remain a tested pair |
| Server execution | Process identity and resource paths | No accidental dependence on desktop Drive sync |

Do not equate root existence with release availability. A file hydration problem and a CFG routing problem can produce similar application symptoms; gather both kinds of evidence.

## Procedure E promote and roll back

Development content may contain experiments. Acceptance content must represent the intended production files, layout, permissions and version restrictions. Promote a release as a coherent set: common framework, modified setup modules, workspace adapters, project selectors where changed and standards resources.

1. Freeze the proposed release and generate a manifest of filenames and checksums.
2. Run the acceptance matrix below and record evidence for each case.
3. Obtain release approval from the designated owner and technical acceptance from the product SME.
4. Install the approved content into the production resource branch using the agreed deployment mechanism. Re-resolve ProjectWise folder values and identifiers for the target datasource; do not carry one datasource's bindings into another.
5. Check effective CSB assignments, permissions and root paths. Run a post-deployment smoke test as an ordinary user.
6. Record the actual deployed release and retain the previous manifest and binding information.
7. Monitor initial users and server processing. Halt further waves if standards selection or output differs.

For rollback, close affected application sessions, restore the previous coherent content and its CSB or selector bindings, then repeat the smoke test. Restoring one CFG while leaving incompatible adapters or resources in place is not a complete rollback. Separate workspace rollback from changes users have already saved into design files; restoring the workspace does not undo design-file content.

See Diagram 05 for promotion and rollback gates.

## Acceptance matrix

| Test | Setup | Pass evidence |
|---|---|---|
| Bootstrap | Ordinary account and assigned CSB | Common CFG trace and correct framework root |
| Standard selection | Standard client example | Group, Configuration and workspace match |
| WorkSet selection | Real project with expected CFG | Exact CFG path, name and design root |
| Missing project CFG | Remove or rename in test only | Designed error or approved explicit fallback |
| Missing workspace CFG | Invalid workspace name in test only | Understandable failure |
| Wrong product | Unapproved generation or build | Restriction behaves as specified |
| Unknown version | Detection cannot supply version | Policy handles unknown value deliberately |
| Multi-client | Two client projects in succession | No cross-client standards leakage |
| Nested WorkArea | Child project below parent | Intended project root selected |
| Deep hierarchy | Approved maximum depth | Search resolves or fails predictably |
| Path include | Conflicting test values in child and parent | Actual order and precedence documented |
| Permission denial | Resource read denied in test | Diagnosable failure and no unrelated standards fallback |
| Rendition | Approved server build | Font, line style and print output match |
| iTwin processing | Approved processing configuration | Correct dataset without desktop-only dependency |
| Drive | Fresh profile and cold content | Required local resources available |
| Rollback | Previous released content restored | Known-good paths and output recovered |

## First line support procedure

Ask for the document path, datasource, product name and exact build, ProjectWise Explorer build, launch method, workspace and WorkSet shown, error text and time of failure. Identify whether the problem affects one document, one WorkArea, one user, one product or all users after a release.

Inspect the effective root and common include first. If the common trace is absent, diagnose CSB assignment, application integration or launch route before editing a client dataset. If the common trace is present, inspect the WorkArea root, selector and workspace adapter. Then inspect the WorkSet CFG and resource paths. See Diagram 06.

Useful diagnostics include `_DYNAMIC_CONFIGS`, `_DYNAMIC_MSG_VALIDATION`, `FINDDIR_VALIDATION_MSG` and `INCLUDEINPATH_VALIDATION_MSG`. Search for the first unexpected root or missing include, rather than starting with the final resource error.

A message appended to `_DYNAMIC_MSG_VALIDATION` is not necessarily a hard startup failure and is not proof of a visible user notification. Inspect the final variable and application behaviour. A `%error` debug statement, in contrast, may intentionally interrupt loading.

Use debug switches on an isolated test scope. Record their prior values, reproduce the failure, capture the result and remove them. Do not leave `_DYNAMIC_DEBUG_COMMONPREDEFINED`, `FINDDIR_DEBUG_SUMMARY` or helper error-summary switches enabled in production.

## Troubleshooting reference

| Symptom | First checks | Corrective action |
|---|---|---|
| No DMWF trace | Effective CSB and launch route | Correct assignment or integration before dataset edits |
| Configuration root not found | Group name, sibling roots and Configuration name | Match real folder names; review Drive overrides |
| Workspace CFG not found | Workspace folder and sibling `<Workspace>.cfg` | Correct selection or restore missing CFG |
| Wrong WorkSet | Selected adapter, nest or marker result | Correct hierarchy contract and project CFG mapping |
| Example standards load | Default WorkSet fallback and defaults | Provide project CFG; make expected path an acceptance gate |
| Wrong Organization resources | Common-root Organization switch | Set intentional source before its calculation |
| Version restriction fails | Engine identity and detected version | Correct detection or approved restriction; retain control |
| Drive root not found | Organization name, current-user registry and sign-in | Correct user configuration and sync setup |
| Drive root found but resource missing | Hydration, sync membership and resource filename | Repair content availability; do not assume routing failure |
| Unexpected include override | Child-to-parent sequence and operators | Remove conflict or establish intended precedence |
| Server differs from desktop | Environment flags, service account, fonts and versions | Test server-specific configuration and permissions |
| Repeated workspace comparison changes | Ignore list and dynamic values | Verify supplied ignore-list construction before modifying policy |
| Local option appears ineffective | Active adapter call and exact flag names | Use implemented flags and establish them at the right time |

## Support evidence record

Use the following fields in a ticket: incident ID; timestamp and timezone; datasource; ProjectWise document and WorkArea paths; user and device; product/build; Explorer/build; framework/adapters/dataset releases; effective CSB; final `_USTN_` roots and names; relevant dynamic variables; first failed include; error text; resource or output affected; last known good launch; changes since that launch; reproduction steps; debug changes applied and removed; proposed fix; test result; rollback result if used.

Do not include authentication tokens, passwords or unrelated project data. Path and release evidence usually provide enough context for configuration diagnosis.
