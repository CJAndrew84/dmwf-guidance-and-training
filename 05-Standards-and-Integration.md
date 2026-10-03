# Standards authoring and integration guidance

## A common governance model with separate product implementations

We can use a common release and project-selection model across Bentley and Autodesk applications while preserving each product's native configuration mechanism. DMWF provides the Bentley managed-workspace route reviewed in this pack. A Civil 3D profile and bootstrap route is a separate implementation, not a hidden feature of the supplied template.

This chapter contains recommendations. It does not claim that a corporate environment, Civil 3D bootstrap, deployment automation or standards conversion tool is included in DMWF 24.

## When the client supplies a complete workspace

Preserve the original dataset and record its supported builds. Start by identifying its native framework roots, external dependencies and expected outputs. Adapt root locations through the ProjectWise PWSetup layer, keeping client resource settings intact where possible.

Check whether the client workspace contains scripts that expect a fixed drive letter, a workstation environment variable, a registry entry or a writable installation folder. A resource library may be portable while its startup scripts are not. Test both desktop authoring and required server processing.

Create a small acceptance model or drawing that exercises the dataset's characteristic resources. A list of copied filenames is insufficient evidence that annotation, feature definitions, line styles or print styles behave correctly.

## When the client supplies files without a workspace

Treat the files as source material for an authored dataset. Classify each resource by product, version and role. Decide which belong at Organization, workspace or WorkSet level. Create the native Configuration and workspace CFGs that make those resources discoverable, then add a PWSetup adapter to route them in ProjectWise.

Maintain a resource register containing filename, source revision, purpose, intended configuration variable, destination layer and acceptance test. Resolve duplicated resource names before release. Copying two DGNLIBs with overlapping names into a wildcard path can produce ambiguous behaviour that DMWF cannot resolve as a standards-authoring decision.

## When the client supplies only PDF standards

A PDF is a specification, not an executable workspace. We must translate its requirements into product-native settings and resources with traceability and technical acceptance.

1. Register the authoritative standard, revision, applicability and any contractual additions.
2. Extract testable requirements: naming, units, levels or layers, symbology, annotation, drawing scales, borders, coordinate reference systems and exchange outputs as relevant.
3. Identify requirements enforced by application configuration and those requiring design checks, information management checks or human review.
4. Record unresolved interpretations and obtain a decision from the standards owner. Do not invent a feature code or layer simply to complete a dataset.
5. Build native resources in an isolated authoring workspace. Record each resource's requirement reference and test.
6. Produce representative drawings or models and compare them against the specification. Check plotted and exported output as well as screen appearance.
7. Release the dataset and DMWF adapter as a tested pair. Publish known limitations and requirements that remain outside automated enforcement.

See Diagram 07. This process applies to a National Highways or rail standards-only commission as a project method; this pack does not assert that a particular current agency provides or lacks a workspace.

## Requirements traceability template

| Requirement ID | Source clause and revision | Interpretation | Implemented resource or setting | Verification | Owner and status |
|---|---|---|---|---|---|
| REQ-001 | Enter authoritative reference | Units for designated model type | Seed model units | Inspect created model and exported output | Assigned SME |
| REQ-002 | Enter authoritative reference | Level or layer naming | Native level/layer library | Create and audit representative elements | Assigned SME |
| REQ-003 | Enter authoritative reference | Drawing border and issue data | Border seed and supported metadata mapping | Sample issued sheet | Assigned SME |
| REQ-004 | Enter authoritative reference | Annotation requirements | Annotation library and scale settings | Sample geometry and plotted sheet | Assigned SME |

These rows are examples of record structure, not claims about any particular standard's requirements.

## Corporate and client coexistence

Use corporate content for shared tools, support links, approved utilities and non-client-specific resources. Use client content for deliverable standards. Use project content for approved project-specific additions and exceptions. A discipline dataset can supply specialised resources where the product supports that arrangement.

DMWF's group module can retain Organization content at the common Bentley root while routing the workspace to a client group. This is a useful structural capability, but precedence among overlapping resource names still comes from native configuration and search order. Do not assume the common Organization always overrides the client or that project additions always override both.

For each resource family, record the owner, include order, append/prepend policy and duplicate-name policy. Test one intentional duplicate in a sandbox to confirm actual product behaviour. Avoid global locks on ordinary resource variables unless there is a deliberate reason and the resulting client compatibility has been assessed.

## Civil 3D profile and Drive bootstrap concept

A possible separate Autodesk route is: ProjectWise Workspace Profile selects an AutoCAD profile; that profile points to controlled support content made available through ProjectWise Drive; an approved startup routine stages resources that must be local and configures supported plugin paths. This is an architecture to implement and test, not a validated script delivered in this pack.

Use Autodesk's applicable startup mechanisms deliberately. The established AutoCAD filenames are `acad.lsp` and `acaddoc.lsp`; a file called `autodoc.lsp` would need explicit loading rather than being assumed to be a standard automatic startup file. Confirm actual loading, trust and product-version behaviour in Autodesk's documentation and runtime before implementing the bootstrap. This pack does not provide Autodesk-specific code or a compatibility certification.

The bootstrap should be idempotent: repeated startup with the same release leaves a stable result. It should compare release identity and checksums, copy only the required approved files, retain a recovery path and log actions in a supportable way. Stage a complete release and switch to it only after validation; do not leave a partly copied library active.

Keep user customisation apart from managed files. A deployment must not overwrite unrelated user support files merely because names collide. If a resource requires installation privileges, deploy it through an approved installer or endpoint-management mechanism rather than assuming a per-user startup routine can write into protected application locations.

Test profile selection, required local content, plugin loading, trusted paths, drawing defaults, user overrides and product upgrades independently. A successful support-file copy does not prove Civil 3D's drawing styles or settings are correct. See Diagram 08.

## ProjectWise Drive and Git authoring

Recommendation: author version-controlled workspace content in a normal Git working tree and publish approved releases into ProjectWise through a controlled transfer process. Do not make a Drive sync root and a Git working tree compete over rename, delete and metadata operations during release management.

Where Drive is used for convenient access to managed source files, establish one authoritative editing route, explicit check-out/check-in behaviour and a recovery process. A Git commit does not prove ProjectWise accepted the change, and a successful Drive sync does not prove the committed release is what users received. Compare content and release identity after publication.

A release record should connect Git commit, adapter/dataset version, manifest and datasource installation result. This is a governance recommendation, not a built-in DMWF Git integration.

## Enterprise deployment automation

Automate repeatable import, binding, permission checks and verification only after a manual deployment has passed. Treat folder/document identities as datasource-specific. A reusable deployment manifest should describe logical paths, filenames, release identifiers and assignments; the deployment mechanism resolves target identities.

Use a pilot datasource, then small waves. Record failures by stage: import, CSB binding, selector adaptation, permissions, desktop acceptance and server acceptance. A successful file transfer is only one stage. Preserve per-datasource results and make retries target the failed stage without silently skipping validation.

DMWF 24 does not contain a complete enterprise deployer in this archive. Any deployer must independently demonstrate its import, idempotency, error handling and rollback behaviour.
