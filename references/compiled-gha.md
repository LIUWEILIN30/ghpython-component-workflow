# Deliver Python components directly as a GHA library

Use this route when the requested deliverable is an installable component library. Keep the fast development loop: stable Python source, external loader, automatic typed ports and focused live acceptance. After acceptance, build a real .NET assembly with independently registered component classes and embedded standalone Python sources. **No intermediate `.ghuser` is required.** Do not turn an existing User Object into a plugin by changing its extension.

## Choose the delivery form

| Need | Suitable artifact |
| --- | --- |
| Iterate on an algorithm | Python source and loader |
| Install a component library with stable identities and embedded assets | `.gha` built directly from accepted sources and a manifest |
| Explicitly requested reusable User Object | `.ghuser`; see [packaging-mcp.md](packaging-mcp.md) |
| Preserve connected examples, sliders or an entire working layout | `.gh` alongside the library; automatic group placement requires a separate tested design |
| Rhino dockable parameter panel | `.rhp`; see [rhino-panels.md](rhino-panels.md) |

When the user requests a GHA, proceed with that form without first packaging User Objects. When a component library is requested without a format, prefer direct GHA if the target SDK/runtime can be verified; explain its Python-host dependency. Preserve an explicit request for ghuser or grouped placement.

## Reuse the Python host rather than rewrite accepted algorithms

The proven Rhino 8.20 / Windows / .NET 8 implementation used a C# wrapper derived from `RhinoCodePluginGH.Components.BaseLanguageComponent`, selected `LanguageSpec.Python3`, and assigned embedded source through `RhinoCodePlatform.GH.IScriptComponent.Text`. It used `RhinoCodePluginGH.Parameters.ScriptVariableParam` for ports. These are version-dependent host APIs: inspect the installed assemblies/XML and prototype one component before generating a library. This skill does not bundle a universal GHA compiler or a ready-made wrapper generator.

The compiled C# classes supply registration, stable component GUIDs, metadata, fixed ports, defaults and icons. Rhino's Python 3 host still executes the embedded algorithm. This avoids rewriting working geometry code and preserves the host's data marshalling and script lifecycle. It does not compile Python algorithms to native code, hide their source, or remove the Rhino Python dependency.

1. **Capture the accepted contract.** Build from an explicit manifest: stable logical identity and GUID, name/category/subcategory, authoritative source, input/output specs, icon and intentional dependencies. Preserve GUIDs for compatible updates; do not derive new identities from release version numbers. A library identity differs from its component identities and each placed instance identity.
2. **Export standalone sources.** Embed the required packet definitions, selected shared helpers and business code at build time. Remove the development loader, workspace imports and test-only exports. Keep development files unchanged; use a distinct build artifact for release adaptations. Custom packets should communicate through their data contract rather than the identity of separately embedded Python classes.
3. **Register genuine component classes.** Give every component an independently discoverable public class and stable GUID. Set names, categories and embedded icons, and register the category icon through `GH_AssemblyPriority` when needed. The tested wrapper configured fixed ports, called `Params.OnParametersChanged`, `VariableParameterMaintenance` and `SetParametersToScript`, enabled input/output marshalling, and then assigned script text. Check the required sequence on the installed SDK rather than assuming every version has the same interface.
4. **Make the compiled contract authoritative.** Configure Name/NickName, Description, Access, input Optional and Type Hint on both sides from the same specs. Disable or adapt only the development-time dynamic port updater in the exported source so it cannot rebuild registered fixed ports during solving. Select requested hints explicitly; fail on unsupported mappings. Verify packet/tree transfer to downstream components.
5. **Embed real defaults.** Put fixed defaults from the accepted source into input persistent data using the corresponding GH goo type. Retain dynamic defaults in Python and describe their dependency (for example, a distance following another input). Do not infer defaults from arbitrary old canvas values, invent geometry, or weaken required inputs. For test replacement, clear persistent data first: `SetPersistentData` can append. Runtime code must preserve user edits and connected inputs instead of restoring defaults on every solve.
6. **Keep descriptions and preview working.** Selecting Type Hints or configuring script parameters can overwrite Description. The tested adapter restored authoritative descriptions after setup, after archive readback and around solves; verify tooltips on fresh, solved and reopened instances. Adapt custom preview to the Python SDK script lifecycle when needed; check drawing and clipping bounds, not just output data. Restrict adaptations to identified packaging functions and compare business-function ASTs without masking arbitrary strings.
7. **Compile with explicit resources and references.** Use a matching .NET SDK project, or installed Roslyn through an authorized in-Rhino execution tool. The tested Roslyn route emitted a dynamically linked library with Python sources, component icons and the category icon as manifest resources. Resolve reference locations against the target installation, collect compiler diagnostics and verify resource membership. No author-specific compiler path or external build service is required. Avoid overwriting an assembly already loaded in Rhino; use a fresh build path and a clean process for final verification.

## Accept the final plugin

First prove the wrapper on one small representative component, including a custom packet or preview where relevant. Then generate the library from the manifest and validate the **final disk GHA**, not just an earlier in-memory assembly:

- Load through Grasshopper's component server and check library registration, exact component membership, unique stable GUIDs, fresh instances, category/component icons, source resource hashes, ports and defaults.
- Exercise real producer/editor/consumer chains, meaningful tree paths, representative failures, repeated solves and any preview. Check descriptions again after script configuration and solving.
- Save and reopen a `.gh` containing the compiled components. Explicitly retrigger Run-controlled components whose cache is not serialized; record that requirement rather than claiming automatic cache recovery.
- Block reads of the development and skill directories without deleting them, then solve again. Embedded source/resources must suffice apart from declared host/runtime dependencies. Compare expected source under a stated newline-normalization rule while keeping binary hashes over raw bytes.
- Package only manifest-selected files, check archive membership/CRC/hashes, and report build, live solve, save/reopen, isolation and installation evidence separately. Keep uploading, machine execution and other side effects outside acceptance unless requested.

For direct manual installation, use GH's File → Special Folders → Components Folder. Identify and back up obsolete same-library ghuser entries and toolbar-only assemblies when an authorized migration would otherwise create duplicate entries. Do not remove unrelated libraries. A new GHA does not automatically convert old Python/ghuser canvas instances: supply a migration plan or a connected example and test new instances. Existing loaded assemblies normally require a Rhino restart; installation and replacing the user's library remain separate from preparing the release.

## Evidence and portability limit

The source experience was NEWPATH 1.0.4: 26 independently registered components in one GHA, 53 embedded Python/icon resources, 104 described inputs and 50 fixed persistent defaults. Windows Rhino 8.20 / .NET 8 tests covered registration, real chains, descriptions after solve/archive readback, save/reopen with Run retriggering, and blocked development-directory reads. Its .NET Framework startup mode, Mac compatibility and post-restart installation were **not** validated. These are historical results, not acceptance of a newly built plugin, nor proof that the panel's dual-runtime success applies to this Python wrapper.
