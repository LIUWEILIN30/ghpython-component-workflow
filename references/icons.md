# Component icons

Use for requested icon design, ghuser icon embedding or icon-only maintenance. Icons are optional and must not change accepted source, ports or component identities.

## Design and mapping

Read each component's purpose from its source header. Use a source stem or stable identity to map each released component to its icon; translated display names and category order are weak keys. Check against the current release manifest, not a historical development inventory that may contain obsolete or omit candidate components.

Maintain an editable SVG master and a gallery showing both enlarged and native-size icons. A 24 by 24 canvas with transparent background was used successfully for GH User Objects; confirm the target runtime's size expectations. Keep silhouettes and visual weight consistent, and use a small shared palette. Distinct functional motifs should remain recognizable at native size. Preview light/dark backgrounds; avoid tiny labels and details that vanish after rasterization.

For editable vector delivery, use real paths/shapes; an embedded bitmap is not an editable vector. Avoid unintended external image/font dependencies. Render a PNG at the target size using a confirmed available SVG renderer, then check dimensions, transparency, clipping and component-to-file coverage. Raster-only artwork is acceptable when explicitly requested; AI raster generation is not needed for simple vector icons.

## Semantic design before rendering

Before drawing, record the source-backed function as input → operation → output. Select one main object and one distinguishing relationship. Separate actual path geometry, surface shading, visible edge bands, and organizational symbols; never infer a component's algorithm from its old icon.

Choose projection per component, not for the entire library. Use restrained 3D when depth, layers, or spatial adjacency explain the function. Use 2D when projection would overlap paths, hide topology, or crowd a 24px canvas. Shared palette, weight, and spacing provide family consistency without forcing every icon into 3D.

A visible outline is a visual effect, not necessarily an SVG stroke. For partly occluded borders, use filled silhouette bands, compound paths, clipping, or back-to-front face layering. Hide rear edges under foreground geometry; do not outline every face independently or double shared edges. Use strokes for actual open paths and simple unobscured contours when appropriate. A raster reference cannot establish how the original vector was authored.

Use a bracket for grouping, an arrow for an actual direction or transformation, and import/export symbols for actual data transfer. Keep one symbol family subordinate to the main object and separated from it by visible space. Do not let a grouping bracket look like a path connection. Remove decorative symbols that do not distinguish function.

Review at native 24px before polishing an enlarged version: main object, operation, gaps, occlusion, and distinction from adjacent component icons. If dense, simplify or switch projection before increasing resolution. Deliver an editable master, transparent native raster, light/dark preview, and a short source-backed semantic note. Candidate drawings remain candidates until accepted; distinguish offline rendering from GH toolbar validation.

## Embed and verify

For first packaging, set the icon through the installed `GH_UserObject` SDK interface before saving; confirm supported members against the installed SDK. The PNG is embedded in the ghuser; the recipient should not require the SVG or PNG file to place the component.

For an authorized icon-only replacement of an existing archive:

1. Preserve baseline ghuser files and their hashes. Read using the installed `GH_IO` archive API and inspect the actual schema.
2. Change only the verified icon item. In the tested archive layout, this was the outer `Icon` stream; do not assume the same XPath for every runtime or object kind. If editing serialized XML, update the stream length as well as base64 bytes and use GH_IO to serialize back to binary.
3. Read back the saved binary. Compare decoded embedded icon bytes with the intended PNG. Normalize only the changed icon item and compare the remaining archive against the baseline, including the embedded Object payload, source, ports, identity and metadata. Unexpected schema or non-icon differences stop the replacement.
4. Verify the release set and update its manifest/ZIP. Keep any intentionally excluded obsolete items out of the archive and installation.
5. Compare authorized installed replacements with release hashes. Save work and fully restart Rhino/GH when required to clear cached library icons. Do not use `ComponentServer.LoadExternalFiles` as a casual refresh: it can reload assemblies. Inspect a newly placed component; old canvas instances are independent copies.

Report offline icon/archive checks separately from toolbar appearance and fresh-instance execution. An icon update does not require unrelated geometry regressions when exact non-icon payload preservation is established; it also cannot convert an earlier runtime result into current live evidence.
