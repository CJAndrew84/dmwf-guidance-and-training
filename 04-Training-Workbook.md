# DMWF training workbook and facilitator guide

## Framework release and application versions

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

**Trainer check:** ask participants why a supplied `24.00` workspace check can reject a supported 25.xx product. Expected answer: the example adapter enforces a workspace policy; it does not define DMWF support. Ask how a 26.xx deployment addresses the missing registry key. Expected answer: configure an Application CSV with the actual product version, lock it before DMWF processing and verify the effective version, selection and drawing output.

## Training purpose

By the end of this course, a participant should be able to explain a DMWF startup, deploy the standard example in a sandbox, select a client workspace, resolve multiple WorkSets and diagnose a wrong standards source. Participants should be able to show evidence for a selection rather than accepting a successful application launch as proof.

This is an administrator and workspace-maintainer course. Designers can attend the foundation session and user verification exercise. Production deployment remains subject to the organisation's usual authority and release process.

## Delivery plan

| Module | Time | Audience | Learning outcome |
|---|---|---|---|
| 1 context and architecture | 35 minutes | All | Distinguish Configuration, workspace, WorkArea and WorkSet |
| 2 configuration language and routing | 40 minutes | Administrators and maintainers | Explain include timing, defaults and locks |
| 3 basic deployment lab | 60 minutes | Administrators | Establish bootstrap and verify final roots |
| 4 client and package selection | 60 minutes | Maintainers | Select explicit adapters and resolve package CFGs |
| 5 diagnostics and recovery | 60 minutes | Support and administrators | Find first wrong root and recover from a failed release |
| 6 Drive and integration decisions | 35 minutes | Maintainers and architects | Separate local content availability from selection |
| Assessment and discussion | 30 minutes | All technical participants | Demonstrate evidence-based diagnosis |

Allow additional breaks and setup time. A two-session delivery works well: foundations and standard setup first, advanced selection and support second. This is a trainer plan, not a delivered Bentley certification programme.

## Facilitator preparation

Prepare an isolated ProjectWise datasource or agreed sandbox scope with an approved Bentley product and integration. Obtain the supplied ZIP and preserve its baseline. Give participants ordinary user accounts for verification and appropriately scoped administration accounts for setup. Prepare two package folders, representative drawings and known expected resource paths.

Create a separate fault-injection copy of the test setup. Prepare faults by changing only one item at a time: wrong Configuration name, missing WorkSet CFG, incorrect nest depth, duplicate marker, disabled CSB assignment and missing Drive resource. Record the original values so each group can reset safely. Never use a production project for fault injection.

Demonstrate Diagram 01 slowly. Stop after each include boundary and ask what information is now available. Explain that the arrows describe a CFG call sequence and its decisions, not a remote API transaction.

## Lab 1 trace a configuration without launching it

**Goal:** understand the package structure before modifying it.

1. Find `Common_Predefined.cfg` and its common PWSetup include.
2. Find the module that supplies datasource defaults and identify the actual advanced-discovery assignment.
3. Locate the WorkArea selector for the client's standard `CONNECTExample`.
4. Write the expected client workspace path from its three selection names.
5. Locate the selected workspace adapter and identify where it looks for WorkSet CFG files.
6. Locate the final workspace CFG and one project WorkSet CFG.

**Submit:** a six-step written load trace, the three client selection names, the expected workspace CFG path and the expected WorkSet collection.

**Expected answer:** the client example selects `ClientName`, `Configuration2024` and `CEWorkspaceExampleAdv`; its workspace CFG is in that Configuration's `Workspaces` collection. The standard adapter first considers the resolved WorkArea's `_PWSetup/WorkSets/`, then the workspace `WorkSets/` collection. The delivered default advanced-discovery assignment is `=0`, not the older workbook value of `1`.

**Discussion:** why is a workbook entry not sufficient evidence of the active default? It can describe an older release or inactive example, whereas the loaded assignment and its position determine the runtime value.

## Lab 2 deploy the standard client example

**Goal:** establish a managed startup with evidence.

Follow Procedure A in the administrator runbook. Import both framework branches, create the Predefined bootstrap using a ProjectWise Folder value, make the test project a WorkArea and verify its selector names.

Capture `_DYNAMIC_DATASOURCE_BENTLEYROOT`, `_DYNAMIC_WORKAREAROOT`, `_DYNAMIC_WORKSPACEGROUPROOT`, `_USTN_CONFIGURATION`, `_USTN_WORKSPACENAME`, `_USTN_WORKSPACECFG`, `_USTN_WORKSETNAME`, `_USTN_WORKSETCFG` and `_USTN_WORKSETROOT`. Identify a loaded standard resource and its source.

**Submit:** the captured values, a screenshot or diagnostic export of the trace, and one verified output example.

**Success criteria:** expected paths and resources, correct ordinary-user access and no unexplained fallback.

**Facilitator answer:** values are environment-specific, so do not grade by matching a literal datasource path. Grade against the participant's declared deployment layout. Fail a result that opens with an unintended default WorkSet.

## Lab 3 diagnose the Configuration name fault

**Goal:** find the first incorrect selection instead of bypassing checks.

In the fault copy, change the project's client Configuration selection from `Configuration2024` to `Configuration2023`, while leaving the installed folder unchanged. Launch the same test document. Capture the error and first incorrect expanded path. Restore the original value and retest.

**Submit:** the changed variable, relevant include or root failure, correction and successful final path.

**Expected answer:** the selector points at a nonexistent client Configuration. Correct the name to the actual folder; do not disable validation or create an empty folder simply to suppress the error. This mirrors the inconsistency in the published installation example.

## Lab 4 resolve two packages using nest depth

**Goal:** show the relationship between path structure and WorkSet name.

Use the supplied client FindByNest project as the starting example. Inspect its package structure and `1234.cfg` and `5678.cfg`. Select the matching nest adapter. Open one representative drawing in each package and capture the selected name, relative subpath and CFG.

In a separate test branch, insert an additional container folder in one package's hierarchy. Predict which folder the same nest depth will select, then verify it. Reset the branch afterward.

**Submit:** two successful package resolutions, the prediction for the changed hierarchy and actual observed result.

**Expected answer:** the selected child depth is relative to the found WorkArea root, not relative to the drawing file alone. A fixed depth is reliable only while the hierarchy contract remains stable. A mismatched selected name can lead to default CFG fallback, so inspect the exact CFG even if startup succeeds.

## Lab 5 compare marker and path include behaviour

**Goal:** distinguish search from include processing.

Inspect the FindByExists adapter's root and nested marker parameters. In a test copy, provide a distinctive marker and show which package it resolves. Introduce a duplicate marker in an unexpected ancestor and record the actual result.

Then use a Plus Include copy. Put `TRAINING_CONTEXT = Child` in an approved matching lower-folder CFG and `TRAINING_CONTEXT = Parent` in a matching parent CFG. Enable continued search through the defined boundary. Predict and observe the final value. Remove the temporary variable and files after the exercise.

**Submit:** marker contract, duplicate-marker result, ordered include trace and final training value.

**Expected answer:** marker matching follows the implemented ancestor search; duplicates are ambiguous without an agreed contract. IncludeInPath processes lower/current folders before ancestors. With plain `=` assignments and continued processing, a later parent assignment can leave the value as `Parent`. If an assignment is locked or uses a different operator, explain the resulting difference.

## Lab 6 diagnose a Drive resource problem

**Goal:** separate local root discovery from content readiness.

Use a sandbox setup with Drive enabled and an approved local resource. Record the organization root and `_PROJECTWISE_DRIVE_FOUND`. In a test copy, remove access to one required local resource while retaining the root folder. Launch and record the result. Restore the resource and repeat.

**Submit:** root-found result, missing-resource evidence, actual resolved search path and corrective action.

**Expected answer:** `_PROJECTWISE_DRIVE_FOUND=1` proves that the module found an existing root, not that the required release content is present. Correct hydration, sync membership, permissions or the resource deployment as appropriate. Do not change the project workspace merely because one file is unavailable locally.

If Drive cannot be made available in the training environment, perform this as a tabletop exercise using the module's branches and a sample support record. Mark the exercise as conceptual rather than pretending it was executed.

## Lab 7 recover a failed release

**Goal:** practice rollback of a coherent release.

Create two sandbox releases with manifests. In the second, deliberately change one adapter to select a nonexistent project CFG while retaining its associated resource release. Attempt the launch and capture the selection evidence. Restore the first release's content and bindings, then rerun the ordinary-user smoke test.

**Submit:** failed release identity, restored release identity, affected files and bindings, final root and successful output.

**Expected answer:** rollback must restore the compatible framework, adapter and dataset set, including changed assignments. Restoring a single root variable is insufficient if the resource content or selector remains incompatible. Design-file changes made during the failed release require their own recovery decision.

## Assessment questions and model answers

1. **What is the smallest useful bootstrap?** A correctly assigned Predefined CSB containing the common framework root and the common CFG include, with readable installed content and functioning product integration.
2. **Why can a correct CSB still have no effect?** It may not be part of the document's effective inherited configuration, or the launch route may not process the expected managed integration.
3. **Can one WorkArea contain several WorkSets?** Yes. The adapters can derive package selection from folder names or markers and choose a package CFG.
4. **Why does a later `:` line not always alter a variable?** It is a default assignment; a prior definition can remain in force.
5. **Where should normal resource settings go?** In the appropriate native Organization, workspace or WorkSet standards layer, with PWSetup principally adapting paths and selection.
6. **Does a validation message always stop startup?** No. Appending to `_DYNAMIC_MSG_VALIDATION` differs from raising `%error`.
7. **What is the risk of a WorkSet default?** It can allow startup with unintended standards when the real project CFG is missing.
8. **What does Drive root discovery prove?** That a root path exists according to the module, not that all release files are accessible or hydrated.
9. **Does `ACADConfiguration` configure Civil 3D?** The supplied example is a Bentley DWG rendition configuration. An Autodesk deployment route needs separate implementation.
10. **Which evidence distinguishes two similarly named datasets?** Expanded source paths and release/file hashes, followed by output validation.
11. **Why must Plus Include order be tested?** It walks from the current folder to ancestors; later parent assignments can change final values.
12. **What should happen with a product version of `00.00.00.00`?** Treat it as unknown detection and follow an explicit compatibility policy.

## Competency rubric

| Criterion | Points | Full-credit evidence |
|---|---|---|
| Context explanation | 15 | Correctly distinguishes WorkArea, workspace and WorkSet |
| Bootstrap setup | 20 | Root, include, effective assignment and ordinary-user proof |
| Selection evidence | 20 | Exact workspace and WorkSet CFG and working roots |
| Fault diagnosis | 20 | Finds first wrong input and fixes cause |
| Advanced method | 15 | Explains nest or marker contract and includes a negative test |
| Release recovery | 10 | Coherent manifest, bindings and verified rollback |

Recommended pass threshold: 80 out of 100, plus mandatory correct workspace selection and removal of temporary debug settings. A participant who bypasses compatibility checks without an approved reason must repeat the relevant exercise, regardless of total points.

## Designer verification exercise

Ask a designer to launch the agreed project document from ProjectWise, confirm the expected workspace and WorkSet, select a named standard resource and produce one sample output. The designer should report a mismatch with document path, product build, selected names and resource/output affected. They should not edit shared PWSetup files to fix a local symptom.

## Facilitator closing discussion

Ask each participant to explain one successful startup and one failure using the same evidence chain. Collect unanswered product-specific questions into the acceptance backlog. Publish the tested deployment record alongside the course material so future trainees can distinguish a teaching example from the organisation's approved configuration.
