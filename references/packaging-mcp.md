# RhinoMCP and portable User Object delivery

This reference applies to explicitly requested `.ghuser` packaging. For an installable component library, use [direct GHA delivery](compiled-gha.md) without a User Object intermediate. For companion Rhino parameter panels, use [rhino-panels.md](rhino-panels.md).

For library membership, file responsibilities and delivery stages, read [workflow-and-artifacts.md](workflow-and-artifacts.md). For requested custom artwork, prepare the semantic brief in [icons.md](icons.md) and use `oil-icon`. For icon-only replacement, use the section below. These stages do not require repackaging unrelated components.

## Bridge identity and capability

The relevant bridge name is **RhinoMCP**, with a reference implementation at [jingcheng-chen/rhinomcp](https://github.com/jingcheng-chen/rhinomcp). An AI client may register it as `rhino`; server name and executable spelling alone do not prove the installed fork/version. Inspect the available tools/capabilities and bridge version instead of assuming another MCP project's tool names.

The reference project documents native Rhino/Python/C# execution, Grasshopper canvas operations, output reading and solving. Start its Rhino bridge with `mcpstart` when that installation requires it. Keep installation separate from an already working connection; do not reinstall on every task. Other bridges are acceptable if they provide the required operations.

Packaging does **not** require an imagined `package_ghuser` MCP tool. With an in-Rhino execution capability, run official Grasshopper SDK serialization code in the correct UI/document context. If only geometry commands are exposed, this is insufficient; provide the prepared local packaging script or the GH Create User Object UI route. Do not bypass client permissions by writing directly to the bridge transport.

## Finish the development artifact first

Validate representative components and their full producer→editor→consumer workflow, including actual inputs, paths, geometry and failure behavior. Once tests are complete, ask the user whether to package and confirm the workflow name and output directory, unless the session already specifies them. Suggest grouping by the workflow rather than placing every unrelated component in one global release directory. Do not ask again for already confirmed release choices.

Prepare final standalone source under `tests/<workflow>/packaging/`. Embed all required configuration/helpers and business code, remove the development loader and pure test exports/Debug, and leave the accepted algorithm unchanged. Document intentional dependencies; default shareable User Objects must not depend on the author's workspace, skill assets or absolute paths.

## Verify Chinese-version text

Apply the language contract in `SKILL.md` to both the User Object's library metadata and its embedded component. Check component names and one-sentence descriptions, every input/output description, the embedded source header and authored error/warning messages. Update authoritative port specs as well as serialized descriptions so a subsequent solve does not restore generic English text. Keep names/nicknames of ports, type hints, access, Optional flags, identities, icons and algorithm behavior unchanged during a text-only update.

Read back the final `.ghuser` and instantiate a fresh object; inspect library/component metadata and all port descriptions after configuration and another solve. Check the source header against actual ports and exercise one representative authored failure. Report remaining GH/Python host-generated English messages explicitly; unchanged host text is not evidence that component-authored diagnostics were translated. If only offline readback was possible, distinguish that evidence from live tooltip/runtime validation.

## Serialize the correct object

Use official [GH_UserObject](https://developer.rhino3d.com/api/grasshopper/html/T_Grasshopper_Kernel_GH_UserObject.htm) and [its methods](https://developer.rhino3d.com/api/grasshopper/html/Methods_T_Grasshopper_Kernel_GH_UserObject.htm), checking signatures against the installed Rhino 8 Grasshopper SDK. Online documentation can describe a newer Rhino version.

1. Create a **fresh** Python 3 component containing final source through the supported script interface for that Rhino version. Do not assume a universal `.Code` property. Configure ports, solve and verify independently of the development component.
2. Inspect a `GH_UserObject` instance and set identity, description/category/subcategory and icon using the installed SDK's supported members. Use distinct stable identities for unrelated components; intentional updates follow the user's release policy.
3. `SetDataFromObject` captures the prepared component. Set the intended output path and call the installed `SaveToFile` overload. Check its return value, file existence and nonzero size. It may overwrite an existing file: preflight the release path and preserve the old artifact unless replacement was authorized.
4. `ReadFromFile`/`InstantiateObject` restore a fresh object for verification. Add it to a clean test definition and confirm ports, geometry, connections, repeated solves and save/reopen. Temporarily make the development source unavailable to establish independence, without deleting the source.
5. Copy verified User Objects and their standalone source to the agreed workflow delivery directory. Include an example `.gh` connecting the components and a dependency/runtime list. Keep per-component usage/port guidance in each source header; workflow notes link to those sources instead of creating duplicate component manuals.

A set of `.ghuser` files delivers reusable individual components. A whole canvas with connections is delivered as `.gh`; a one-node encapsulated workflow requires a tested cluster or other explicit design. `.ghuser` packaging is not compilation to `.gha` and does not guarantee source-code secrecy. Do not silently change the chosen delivery form.

To install User Objects, users can locate GH's User Objects directory via File → Special Folders → User Objects Folder, copy the intended artifacts there, and refresh/restart GH as needed. Verify on the recipient's supported runtime; copied files alone are not proof of working installation.

## Icon embedding and icon-only updates

Use accepted PNG artwork produced through `oil-icon`; artwork creation is described by that skill. Confirm the installed runtime's required image dimensions and prepare the embedded size from the accepted image. This does not require an SVG master or a pixel-grid design workflow.

For first packaging, set the icon through the installed `GH_UserObject` SDK interface before saving. Confirm supported members against the installed SDK. Embed the image so recipients do not need external artwork files.

For an authorized icon-only update, preserve baseline ghuser files and hashes, then inspect the actual schema using the installed GH_IO API. Update only the verified icon fields. A library icon and an embedded component icon may be separate: the tested NEWPATH layout used outer `Icon` and inner `IconOverride`. Do not assume one field updates both or hard-code an unverified schema. When editing serialized streams, update lengths and serialize back through GH_IO.

Read back the final binary and verify all intended embedded images against the accepted PNG bytes or decoded RGBA pixels, as appropriate to the serialization. Compare the remaining archive content against the baseline after excluding only the intended icon fields; preserve source, ports, identities and unrelated metadata. Resolve unexpected differences before delivery. Update manifest hashes and the requested archive; compare any authorized installed copies with release hashes.

Report offline archive checks separately from live toolbar appearance and fresh-instance execution. Save work and restart Rhino/Grasshopper when needed to clear caches. Avoid assembly-loading APIs as a casual toolbar refresh. Existing canvas instances retain their own state; inspect a newly placed component. Exact preservation of non-icon content does not establish new live execution evidence.
