# DMWF quick reference and operating templates

## Application support quick reference

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

For a 26.xx release record, capture the application name, installed full build, Application CSV location, specified and locked version, effective DMWF version values and acceptance-test evidence. Review the locked value after every application update.

## Administrator quick reference

**Bootstrap:** assigned Predefined CSB → ProjectWise Folder root → String include of `Common_Predefined.cfg`.

**Project selection:** WorkArea root → `_PWSetup/WorkAreaPWSetup_Predefined*.cfg` → client group, Configuration, workspace and adapter.

**First four values to inspect:** `_DYNAMIC_WORKAREAROOT`, `_USTN_WORKSPACECFG`, `_USTN_WORKSETCFG`, `_USTN_WORKSETROOT`.

**First trace to inspect:** `_DYNAMIC_CONFIGS`. If it does not show the common configuration, investigate effective CSB and launch integration.

**Expected output check:** selected resource source path, application behaviour and representative plotted/exported output.

**Common mistakes:** using `Configuration2023` for the supplied `Configuration2024` branch; accepting default WorkSet fallback; confusing WorkArea and WorkSet; changing a setting after the consuming module ran; assuming Drive root existence proves content readiness; treating `ACADConfiguration` as a Civil 3D deployment package.

## Project selection record

| Field | Value to complete |
|---|---|
| Project and datasource | |
| WorkArea path and name | |
| Framework release and root | |
| Client group | |
| Configuration name and root | |
| Workspace name and CFG | |
| Workspace adapter filename | |
| Selection method and hierarchy contract | |
| WorkSet name and CFG | |
| WorkSet root and Standards root | |
| DGNWS location and permissions | |
| Approved desktop and server builds | |
| Drive policy and organization | |
| Owner and approval date | |
| Acceptance evidence | |

## Release record

| Field | Value to complete |
|---|---|
| Release ID and date | |
| Git commit if used | |
| Vendor baseline checksum | |
| Framework and adapter revisions | |
| Standards dataset revision | |
| Local patch IDs and reasons | |
| File manifest | |
| Target datasources and roots | |
| Changed CSB bindings or project selectors | |
| Acceptance test result | |
| Approver | |
| Previous known-good release | |
| Rollback procedure and evidence | |

## Change request

Describe the project need, current behaviour, intended behaviour and affected products. Identify the smallest configuration layer that can implement the change. List the files and variables affected, their include timing, locks and resource precedence. Attach a positive test, a negative test and rollback steps. Record whether the change affects all clients, one workspace or one WorkArea.

## Standards exception record

| Field | Value to complete |
|---|---|
| Exception ID | |
| Standard and clause | |
| Project scope | |
| Reason and agreed interpretation | |
| Resource or variable implementing exception | |
| Approval and expiry or review date | |
| Verification evidence | |

## Release checklist

- Framework and resource manifest frozen.
- Exact workspace and WorkSet selection verified.
- Unknown and unsupported product versions tested.
- Required ordinary-user read and project write access verified.
- Missing-resource and fallback behaviour documented.
- Desktop output accepted by product SME.
- Required server processing accepted.
- Drive cold-content behaviour tested if used.
- Debug and fault-injection settings removed.
- Rollback target and bindings recorded.
- Deployment and post-release evidence retained for each datasource.

## Support handoff

Provide the incident scope, last known-good release, first wrong expanded path, applicable selector/adapter, captured trace and reproducing document location. State the next decision needed: access correction, path correction, dataset change, compatibility decision or vendor clarification. This keeps support focused on the cause rather than a series of speculative root overrides.
