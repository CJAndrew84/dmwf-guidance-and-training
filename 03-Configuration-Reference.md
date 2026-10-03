# DMWF configuration reference

## Reading this reference

This is a curated reference for the supplied 24.0.0.0 package. For exact assignment locations, operators and source line numbers, use `09-Variable-Occurrence-Index.md`. That generated index includes active assignment statements across all supplied CFGs, including documentation templates and alternate examples. An occurrence in the index does not prove that the file loads for a particular project.

The delivered workbook is useful for understanding structure but contains older variable names, V8 entries and values that differ from active 24 defaults. Prefer the delivered implementation for this package, then verify its runtime result. Preserve exact spelling when a variable is consumed by code, including the delivered `_DYNAMIC_PRODUCT_ONE_VARIBLES` spelling.

## Names and roots

| Variable | Role | Usual source or consumer |
|---|---|---|
| `_DYNAMIC_DATASOURCE_BENTLEYROOT` | Root of common framework | Predefined CSB; consumed throughout |
| `_DYNAMIC_DATASOURCE` | Current datasource Documents context | Common file assigns `@:` |
| `_DGNDIR` | Context path for the opened design file | Application/integration input |
| `_DYNAMIC_CONNECTEDPROJECT` | Connected project root if found | Common file using `DMS_CONNECTEDPROJECT` |
| `_DYNAMIC_WORKAREA` | WorkArea context for opened file | Common file using `DMS_PROJECT` |
| `_DYNAMIC_PARENTWORKAREA` | Parent WorkArea context | Common file using `DMS_PARENTPROJECT` |
| `_DYNAMIC_WORKAREAROOT` | Root chosen for project setup | Common setup discovery |
| `_DYNAMIC_WORKAREAROOT_NAME` | Chosen root's name | Used for default WorkSet selection |
| `_DYNAMIC_PWSETUP_PATH` | General setup-folder path | Common default `_PWSetup/` |
| `_DYNAMIC_WORKAREA_PWSETUP_PATH` | WorkArea-specific setup path | Datasource default `_PWSetup/` |
| `_DYNAMIC_WORKSPACEGROUPNAME` | Client or group selection | WorkArea selector |
| `_DYNAMIC_WORKSPACEGROUPSROOT` | Collection of groups | Group module or explicit configuration |
| `_DYNAMIC_WORKSPACEGROUPROOT` | Selected group root | Group module; optionally Drive |
| `_DYNAMIC_WORKSPACEGROUP_CONFIGURATIONNAME` | Configuration folder within group | WorkArea selector or default |
| `_DYNAMIC_CONFIGURATIONNAME` | Common Configuration folder name | Datasource default `Configuration` |
| `_DYNAMIC_CONFIGURATIONROOT` | Selected Configuration root | Group processing or Drive/local adaptation |
| `_DYNAMIC_CONFIGURATIONROOT_PWSETUP` | Configuration adaptation root | Group/common calculations |
| `_DYNAMIC_CEORGANIZATIONROOT` | Organization standards root | Group/common calculations |
| `_DYNAMIC_CEWORKSPACESROOT` | Workspace collection | Group/common calculations |
| `_DYNAMIC_CEWORKSPACENAME` | Selected workspace name | WorkArea selector or default |
| `_DYNAMIC_CEWORKSPACEROOT` | Selected workspace content root | Common calculations |
| `_DYNAMIC_CEWORKSPACEROOT_PWSETUP` | Selected workspace adapter root | Common/group calculations |
| `_DYNAMIC_WORKSPACECFG` | Workspace CFG file | Common framework |

Verify root variables as expanded paths, not just their definitions. A correct-looking definition can expand through a wrong group or Drive root. Root variables and root-name variables are not interchangeable.

## WorkSet selection and working content

| Variable | Role | Standard example behaviour |
|---|---|---|
| `_DYNAMIC_WORKSPACEPWSETUP_PREDEFINED_NAME` | Workspace adapter filename or pattern | Default `WorkspacePWSetup_Predefined*.cfg` |
| `_DYNAMIC_WORKAREA_CFG_PATH` | Project CFG collection relative to WorkArea | `_PWSetup/WorkSets/` |
| `_DYNAMIC_WORKAREA_CFG_ROOT` | Expanded CFG collection | Project location if present, otherwise workspace collection |
| `_DYNAMIC_WORKSET_NAME` | Intended WorkSet name | WorkArea name or discovered package name |
| `_DYNAMIC_WORKSET_DEFAULTNAME` | Fallback CFG name | Common default `ConnectExample`; adapter example `CONNECTExample` |
| `_DYNAMIC_WORKAREA_SUBPATH` | Relative design/project working location | Empty by default or discovered by helper |
| `_DYNAMIC_WORKAREA_WORKSET_PATH` | Adapter's relative WorkSet path | Defaults from subpath |
| `_DYNAMIC_WORKAREA_WORKSET_ROOT` | Adapter's expanded working root | Project root plus relative path |
| `_USTN_WORKSETSROOT` | WorkSet CFG collection for Bentley | Adapter's CFG root |
| `_USTN_WORKSETNAME` | Selected Bentley WorkSet name | Intended name or fallback |
| `_USTN_WORKSETCFG` | Selected Bentley WorkSet CFG | Existing project CFG or fallback CFG |
| `_USTN_WORKSETROOT` | WorkSet working root | Adapter's working root |
| `_USTN_WORKSETSDGNWSROOT` | DGNWS root setting | Defaults to selected WorkSets collection |
| `_USTN_WORKSETSTANDARDS` | Project standards location | Standard example uses WorkSet root plus `Standards/` |

Do not use Windows case-insensitivity as a naming policy. Use the exact stored folder and document names in release records. Renaming a WorkArea may alter derived WorkSet selection even when the standards themselves have not changed.

## Final application framework settings

| Application variable | DMWF meaning | Check |
|---|---|---|
| `_USTN_CONFIGURATION` | Selected Configuration | Correct managed or approved local root |
| `_USTN_CUSTOM_CONFIGURATION` | Custom Configuration redirection | Matches intended framework route |
| `_USTN_USER_CONFIGURATION` | User Configuration redirection | No unexpected personal dataset |
| `_USTN_ORGANIZATION` | Organization resource layer | Corporate or client source chosen deliberately |
| `_USTN_WORKSPACESROOT` | Collection of workspace CFGs | Contains selected CFG |
| `_USTN_WORKSPACENAME` | Selected workspace name | Matches project requirement |
| `_USTN_WORKSPACECFG` | Selected workspace CFG | Exact expected file |
| `_USTN_WORKSPACEROOT` | Workspace standards content root | Correct client and dataset release |
| `_USTN_CONNECT_PROJECTGUID` | Connected project identity if available | Separate from WorkSet selection |

The common file locks several of these roots and names. An adapter should establish intended values before later locks; normal standards settings should consume them rather than repeatedly trying to redirect them afterward.

## Default switches verified in the package

| Switch or setting | Delivered default | Implication |
|---|---|---|
| `_DYNAMIC_FINDWORKAREAROOT_ADV` | `0`, assigned with `=` in defaults | Advanced discovery is off; earlier values may be replaced |
| `_DYNAMIC_WORKAREA_BY_PWWORKAREA_AND_EXISTS_PATH` | `1` | Used within enabled advanced discovery |
| `_DYNAMIC_WORKAREAROOT_MAXWORKAREANEST` | `4` in defaults | Advanced module supports up to four WorkArea contexts |
| `_DYNAMIC_WORKAREA_BY_ROOT_NAME` | `0` | Name-based advanced root discovery off |
| `_DYNAMIC_WORKAREA_BY_EXISTS` | `0` | Marker-based advanced root discovery off |
| `_DYNAMIC_WORKAREA_BY_DEFAULT_VALUES` | `0` in defaults | Advanced default-root fallback off |
| `_DYNAMIC_WORKAREA_EXISTS_PATH` | `_PWSetup/WorkAreaPWSetup*.cfg` | Marker used for advanced discovery |
| `_DYNAMIC_WORKAREA_PWSETUP_ENABLED` | `1` in coordinator | Reads project selector if root is found |
| `_DYNAMIC_WORKSPACEGROUPS_ENABLED` | `1` | Workspace group processing on |
| `_DYNAMIC_WORKSPACEGROUPSROOT_NAME1` | `ClientWorkspaces/` | First sibling group collection candidate |
| `_DYNAMIC_WORKSPACEGROUPSROOT_NAME2` | `WorkspaceGroups/` | Second sibling candidate |
| `_DYNAMIC_ORGANIZATIONROOT_AT_BENTLEYROOT` | `1` in group module | Organization resources normally remain common |
| `_DYNAMIC_CONFIGURATIONROOT_AT_BENTLEYROOT` | `0` | Group Configuration normally supplies configuration root |
| `_DYNAMIC_PWSETUP_IN_CONFIGURATION` | `1` | Uses calculated in-configuration setup roots |
| `_PROJECTWISE_DRIVE_ENABLED` | `0` | Drive processing opt-in |
| `_PROJECTWISE_DRIVE_REQUIRED` | `0` in Drive module | Missing Drive root not mandatory failure by default |
| `_PROJECTWISE_DRIVE_ENABLE_ERRORS` | `0` | Certain missing paths append validation instead of hard error |
| `_DYNAMIC_CHECK_VERSION` | `1` in standard adapter | Example product checks active |
| `_DYNAMIC_USE_LOCAL_ROOT` | `0` in standard adapter | Local-root helper opt-in |

A module's default can differ from the earlier datasource default. Read the sequence and the operators before deciding which value applies. The automatically enabled OneMapping path for detected Explorer major `10` is another reason not to interpret every visible `:0` as a final disabled value.

## Drive reference

| Variable | Purpose | Operating note |
|---|---|---|
| `_PROJECTWISE_DRIVE_ORG` | Organization key and profile folder name | Sample default must be adapted |
| `_PROJECTWISE_DRIVE_REG` | Registry-supplied organization directory | Per current user |
| `_PROJECTWISE_DRIVE` | Found local Drive root | Registry route sets and locks it |
| `_PROJECTWISE_DRIVE_FOUND` | Root-found flag | Existence signal only |
| `_PROJECTWISE_DRIVE_CONFIGURATIONNAME` | Local Configuration folder selector | Used when group selector is not present |
| `_PROJECTWISE_DRIVE_CONFIGURATIONROOT` | Local Configuration root | Can feed `_DYNAMIC_CONFIGURATIONROOT` |
| `_PROJECTWISE_DRIVE_WORKSPACEGROUPNAME` | Local group folder selector | Has precedence over Configuration selector in module |
| `_PROJECTWISE_DRIVE_WORKSPACEGROUPROOT` | Local group path | Can feed `_DYNAMIC_WORKSPACEGROUPROOT` |
| `_PROJECTWISE_DRIVE_WORKAREANAME` | Local WorkArea folder name | Falls back to resolved WorkArea root name |
| `_PROJECTWISE_DRIVE_WORKAREA` | Local WorkArea path | Verify actual consumers; finding it does not prove all design roots changed |
| `_DYNAMIC_PWDRIVE_PROCESSED` | Processing-complete flag | Prevents repeated coordinator processing |

## Search helpers

`FindNameInPath.cfg` and `FindInPath.cfg` search upward from a starting context. They use temporary `FINDDIR_` variables and default cleanup. Several results remain useful to callers, while parameters and temporary values can be undefined. Copy required values into persistent `_DYNAMIC_` variables before explicitly clearing state or starting another independent search. Inspect the helper's actual cleanup block when troubleshooting missing evidence.

| Parameter or result | Meaning |
|---|---|
| `FINDDIR_SEARCHPATH` | Starting path, normally `_DGNDIR` |
| `FINDDIR_NAME` | Legacy name input used by examples |
| `FINDDIR_FINDNAME` | Enable name matching |
| `FINDDIR_FINDNAME_NAME1` and `NAME2` | Alternate name candidates in FindInPath |
| `FINDDIR_FINDIFEXISTS` | Enable marker existence search |
| `FINDDIR_FINDIFEXISTS_PATH` | Marker path relative to each candidate ancestor |
| `FINDDIR_NEST` | Child depth relative to discovered root, as implemented by helper |
| `FINDDIR_FINDIFEXISTS_NEST` | Enable nested marker route |
| `FINDDIR_FINDIFEXISTS_NEST_PATH` | Marker identifying nested selection |
| `FINDDIR_GETROOTPATH` and `GETNESTPATH` | Request relative path construction |
| `FINDDIR_FOUND` | Search state and found directory position; not only a Boolean |
| `FINDDIR_FINDNAME_FOUND` | Specific successful name-match flag |
| `FINDDIR_FINDIFEXISTS_FOUND` | Specific successful marker-match flag |
| `FINDDIR_FOUNDROOT` | Found ancestor path |
| `FINDDIR_FOUNDNEST_NAME` | Selected nested folder name |
| `FINDDIR_FOUNDNEST_DIR` | Selected nested folder path |
| `FINDDIR_FOUNDNEST_PATH` | Constructed relative path |
| `FINDDIR_SEARCHED` | Search trace path |
| `FINDDIR_UNDEF` | Default cleanup switch |
| `FINDDIR_UNDEF_ONLY` | Cleanup-only mode |
| `FINDDIR_DEBUG_SUMMARY` | Debug mode which can use `%error` |
| `FINDDIR_VALIDATION_MSG` | Helper diagnostic text |

Do not treat a nonzero `FINDDIR_FOUND` as sufficient proof of a valid semantic match. Some source branches assign a position even when ancestry is exhausted. Check the relevant specific match flag and the returned path against the project contract.

## IncludeInPath reference

| Parameter or result | Delivered default | Role |
|---|---|---|
| `INCLUDEINPATH_SEARCHPATH` | `_DGNDIR` | Start of ancestor walk |
| `INCLUDEINPATH_STOPNAME` | Last directory piece of `_DGNDIR` | Stop boundary; override for project use |
| `INCLUDEINPATH_NESTMAX` | `4` | Source traversal gate, not an intuitive exact folder count |
| `INCLUDEINPATH_EXISTSPATH` | `IfExistsPath/Include*.cfg` | Pattern relative to each visited folder |
| `INCLUDEINPATH_CONTINUE` | `0` | Stop after first match unless enabled |
| `INCLUDEINPATH_INCLUDEDEPTH` | Not-found text initially | Tracks visited depth during processing |
| `INCLUDEINPATH_DEBUG_SUMMARY` | `0` | Appends diagnostic detail |
| `INCLUDEINPATH_ERROR_SUMMARY` | `0` | Can stop with error summary |
| `INCLUDEINPATH_UNDEF` | `1` | Clears parameters and temporary values afterward |
| `INCLUDEINPATH_VALIDATION_MSG` | Accumulated text | Record of parameters and included locations |

The source gates first depth with `NESTMAX>2`, second depth with `>3`, third with `>4`, and so on. The supplied Plus Include value of `6` consequently permits up to four implemented positions before other stops, rather than a straightforward six-directory traversal. Test the intended boundary and do not increase the parameter solely to make the number look like the desired depth.

## Product and environment reference

**DMWF 24 names the framework release year (2024), not the Bentley application generation.** DMWF 24.0.0.0 supports Bentley product versions `10.xx`, `23.xx`, `24.xx` and `25.xx`. Technically, `26.xx` products can also use the framework, but the missing registry key prevents the existing version-detection route from identifying their version. For `26.xx`, an **Application CSV is required to specify and lock the product version**. Framework support and the suitability of a particular standards dataset are separate decisions.

For 26.xx, use the Application CSV to supply and lock the product version before this module consumes it. Verify the effective `_DYNAMIC_PRODUCT_VERSION` and `_DYNAMIC_PRODUCT_VERSION_GEN_MAJ`; check the trace rather than assuming the registry route succeeded. Follow the actual application configuration tooling for CSV fields and lock syntax. The supplied version module reads `HKEY_CLASSES_ROOT\Installer\Dependencies\$(MS_PRODUCTCODEGUID)\Version` in its registry branch; the missing key is a detection issue, not a framework generation limit.


| Variable | Purpose |
|---|---|
| `_ENGINENAME` | Product identity used by adapters and mapping |
| `_DYNAMIC_PRODUCT_VERSION` | Detected complete product version |
| `_DYNAMIC_PRODUCT_VERSION_FOUND` | Version detection success flag |
| `_DYNAMIC_PRODUCT_VERSION_GEN_MAJ` | Generation and major string used by example restrictions |
| `_DYNAMIC_PRODUCT_VERSION_PROCESSED` | Product module processing guard |
| `_DYNAMIC_PWE_VERSION` and `_DYNAMIC_PWE_VERSION_MAJ` | Explorer version and mapping decision inputs |
| `_DYNAMIC_ONEMAPPING_ENABLED` | Optional mapping enable switch |
| `_DYNAMIC_PRODUCT_ONE_VARIBLES` | Delivered mapping flag with this exact spelling |
| `_USTN_PRODUCT_ONE_GROUPNAME` and `_USTN_PRODUCT_ONE_SUBGROUPNAME` | Product resource grouping |
| `_DYNAMIC_IS_ITWINSYNCENGINE` | iTwin sync engine detection flag |
| `_DYNAMIC_IS_RADS_JOB` | Rendition job indicator |
| `_DYNAMIC_IS_MSPRINTSERVER` | Print-server process indicator |
| `_DYNAMIC_ISDWGFILE` | DWG extension detection result |
| `_DYNAMIC_OS_IS_WORKSTATION` | Windows workstation detection result |
| `_DYNAMIC_RENDITIONENGINE_CHECK` | Enables rendition detection branch |

## Diagnostic reference

| Variable | Interpretation |
|---|---|
| `_DYNAMIC_CONFIGS` | Ordered trace appended by many delivered CFGs; some labels contain older versions |
| `_DYNAMIC_MSG_VALIDATION` | Accumulated findings and selected roots; can include warnings without stopping |
| `_DYNAMIC_DEBUG_COMMONPREDEFINED` | Common final error-summary debug switch |
| `_DYNAMIC_FINDWORKAREAROOT_DEBUG_ERROR` | Advanced discovery debug error switch |
| `FINDDIR_VALIDATION_MSG` | Search parameter and result evidence |
| `INCLUDEINPATH_VALIDATION_MSG` | Path include evidence |
| `PW_MWP_COMPARISON_IGNORE_LIST` | Workspace comparison exclusions; inspect source anomaly before relying on it |

A trace label is not a reliable installed-release identifier when a file's header or append string is stale. Use file hashes and a release manifest as the authoritative baseline, then use traces to explain load order.
