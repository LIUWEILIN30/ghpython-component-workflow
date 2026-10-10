# Component icon semantics and oil-icon handoff

Use when custom component icons are requested. This skill supplies source-backed component semantics; use the **`oil-icon` skill** for artwork creation. The earlier hand-authored SVG / 24px pixel-icon design approach was unsuccessful and is retired from this workflow. Do not reuse its drawing recipe or require editable SVG masters.

## Understand each component before generating artwork

Read the current source header, port specifications and relevant implementation. Record **input → operation → output**, then select the main object and the relationship that distinguishes this component from its neighbours. Do not infer the algorithm from an old icon or a translated name.

When spatial geometry matters, identify the actual objects (curves, surfaces, solids, layer stacks, walls or ribs), their topology and spatial relationships. Distinguish open from closed paths, inner from outer boundaries, layered geometry from continuous connections, and geometry from data-grouping symbols. Use source geometry or provided views to resolve ambiguity; identify uncertainty instead of inventing hidden geometry. Suggest a 3D metaphor when depth, layers or adjacency explain the operation, or a 2D metaphor when that communicates it more clearly. This is semantic preparation, not a separate rendering method.

Prepare one concise concept per component: stable source stem or identity, displayed name, source-backed function, concrete visual metaphor and any essential spatial relationship. Check coverage against the current release manifest. Use stable identifiers for file mapping rather than category order or translated display names.

## Hand off artwork to oil-icon

Locate **`oil-icon`** in the available skill catalog and read its `SKILL.md`. Pass the component concepts, requested count, accepted visual references and any established style specification. Follow that skill for style selection, prompt construction, image generation, slicing, background removal, visual QA and organized transparent PNG delivery; do not duplicate those procedures here. Reuse the agreed style for later batches.

If `oil-icon` is unavailable, finish the semantic brief and explain that the artwork stage needs that skill installed or made available. Do not silently fall back to the retired SVG / 24px design workflow. Do not install dependencies or generate artwork merely because this reference was opened for a documentation task.

For authorized embedding of accepted PNGs into ghuser files, use [packaging-mcp.md](packaging-mcp.md#icon-embedding-and-icon-only-updates). Runtime icon sizing is a packaging detail, not a mandate to design artwork on a 24px canvas. Keep artwork approval, archive verification and live Grasshopper observation as distinct evidence.
