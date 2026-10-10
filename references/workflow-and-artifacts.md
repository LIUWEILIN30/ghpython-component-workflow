# Development through delivery

Use this map when planning a whole component/workflow delivery, packaging a library, or organizing its files. For an algorithm edit, return directly to the source header and relevant function. Packaging, localization, custom icons and installation are conditional stages: perform those requested or already authorized.

| Stage | Authoritative artifact and action | Evidence before advancing |
| --- | --- | --- |
| Scope and environment | Record runtime, Rhino-readable source path, data contract and requested delivery form. Follow existing project layout. | Known interpreter and path mapping; bridge capabilities only if needed. |
| Develop | Maintain a stable source plus loader; generate helpers, define typed input/output specs, header and algorithm. | Focused logic checks; scheduled port setup followed by a solve. |
| Accept behavior | Exercise realistic inputs and the producer/editor/consumer chain. | Geometry, meaningful tree paths, errors, repeated solves, compatible wires and save/reopen. Distinguish offline and live results. |
| Prepare standalone release | Build a separate source without the workspace loader or test-only exports. Choose component metadata and stable identity. | Accepted behavior retained; no undeclared workspace dependencies. Read [packaging-mcp.md](packaging-mcp.md). |
| Localize if requested | Apply the language contract to authoritative specs/header/diagnostics and both library and embedded metadata. | Readback matches source; descriptions survive port setup and another solve. Localization must not weaken inputs or alter algorithm semantics. |
| Design icons if requested | Prepare component function and spatial-geometry concepts, then use `oil-icon` for artwork and transparent PNG delivery. | Follow `oil-icon` QA and verify component-to-file coverage. Read [icons.md](icons.md). |
| Serialize and check | Package fresh components, or apply a bounded metadata update to recoverable existing artifacts. | Binary readback, exact embedded source, metadata/ports/icon/identity checks; live instantiate and workflow checks when available. |
| Deliver and install | Build an explicit manifest-based archive, source, requested examples and concise runtime/install notes. Install only within authorized scope. | ZIP integrity, exact membership and hashes; installed file matches release; restart and fresh-instance checks when available. |
| Maintain and archive | Keep current source paths stable; archive completed experiments, scripts and baselines with provenance. | Migration map, hashes and repaired active links; reports retain their batch-specific scope. |

If a stage cannot be verified live, finish the useful offline work, record the limitation and concrete remaining acceptance steps. File existence, an earlier solve or archive roundtrip does not substitute for fresh-instance execution.

## File responsibilities

Use these roles within the existing layout; they are not mandatory directory names:

- **Active development:** source, loader, focused tests and necessary fixtures. Do not move sources merely to tidy folders: loaders and external callers may depend on their paths.
- **Current release:** only manifest-selected User Objects, exact embedded standalone sources, released icons, relevant verification evidence and delivery notes. Frozen artifacts follow the project's release policy.
- **Tooling and design:** reusable build/check scripts, accepted icon artwork, shared style specifications, semantic concepts and file mapping. Keep one authoritative accepted image per component; mark release copies as derived snapshots.
- **Evidence:** dated results, tested artifact hashes and whether checks were offline, live, manual or user-reported. Do not rewrite an old report to imply verification of a new build.
- **History:** previous releases, failed packaging attempts, completed one-time transformations, scratch XML and recovery files. Keep a migration index; exclude these from discovery, installation and release ZIPs.

Before reorganization, inspect path references, manifests and callers. Preserve recoverable originals, move within the intended project and compare hashes. Repair current consumers; archived scripts may retain old paths as historical evidence if clearly marked non-current. Avoid adding competing inventories or per-component manuals.

## Release manifest and archive

Use a machine-readable manifest as the release membership authority. Record component/source identity, category/subcategory, package path, standalone source path, relevant hashes and component-to-PNG mapping. Prefer paths relative to the release/project root for portable delivery; preserve established working paths when reorganizing an existing project.

State the hash convention: raw file bytes versus normalized source text (encoding, BOM and newline handling). Preserve an established convention explicitly; a CRLF/LF difference must not be mistaken for an algorithm change or silently accepted without comparison.

Build from that explicit list rather than recursive directory discovery. Reject duplicate package paths or identities, missing/extra artifacts and source/hash mismatches. Include notes and verification files intentionally; exclude backups, caches, scratch files and obsolete components. Reopen the ZIP, check CRCs, exact component membership and archived bytes against disk. Examples from an older build must be labeled with their tested baseline rather than presented as acceptance of the current release.

For an icon-only or text-only update, compare all unrelated serialized fields against the baseline. AST comparison is supporting evidence for localization, not proof of equivalent behavior: masking every string can hide changes to protocol keys, paths or business constants. Define exactly which strings may change and preserve the rest.

Installation targets must be unambiguous. Look for duplicates before replacing an existing User Object, preserve the authorized baseline and compare installed hashes. A newly installed library item and an existing canvas instance have separate embedded state. Restart/refresh by the runtime's supported route; do not blindly invoke assembly-loading APIs as a toolbar refresh.
