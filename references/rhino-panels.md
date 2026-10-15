# Rhino parameter panels with two runtime targets

Use this reference for a Rhino dockable panel that exposes or edits Grasshopper parameters. It covers a companion `.rhp` plugin, not GH canvas menus, dragging, snapping, mouse hooks or other editor-interaction extensions.

## Architecture and fast iteration

Keep accepted Python algorithms and typed GH ports in their existing component workflow. Implement the companion panel in C# using RhinoCommon, the Grasshopper SDK and the chosen UI toolkit. The proven Windows implementation used Windows Forms: a `Rhino.PlugIns.PlugIn` registered a panel via `Rhino.UI.Panels.RegisterPanel`, and a Rhino command opened it via `Panels.OpenPanel`.

Share panel functionality, plugin/panel GUIDs, command identity and embedded icon across runtime builds. A category-icon-only `.gha` belongs to GH registration and is independent of the Rhino panel. Renaming a panel must not silently change stable identities or erase a previously accepted layout/behavior fix. When reconstructing from sources, compare the accepted release baseline before rebuilding.

For an invisible panel, inspect the registered plugin path and actual installed file before blaming the selected .NET runtime. A proven failure was a stale registration pointing to a moved release folder. Confirm plugin load result, runtime, assembly location, panel registration and actual visibility; reflection-based construction alone does not prove a docked panel opens.

## Build one source set for both runtimes

When both Windows Rhino 8 runtime modes are requested, use an SDK-style C# project with `TargetFrameworks` set to `net48;net8.0-windows` for the tested Rhino 8.20+ baseline. Match other requested Rhino releases to their actual runtime; a .NET 8 binary is not a promise of .NET 7 support. Build with a .NET SDK, not merely an installed runtime or the legacy Framework `csc.exe`.

The important project settings are:

```xml
<PropertyGroup>
  <TargetFrameworks>net48;net8.0-windows</TargetFrameworks>
  <UseWindowsForms>true</UseWindowsForms>
  <EnableDefaultItems>false</EnableDefaultItems>
  <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
  <TargetExt>.rhp</TargetExt>
  <RhinoReference>$(RhinoSystem)/RhinoCommon.dll</RhinoReference>
  <RhinoReference Condition="'$(TargetFramework)' == 'net8.0-windows'">$(RhinoSystem)/netcore/RhinoCommon.dll</RhinoReference>
</PropertyGroup>
```

This is a settings excerpt, not a complete buildable project. Supply the real assembly name, source list, embedded icon and installation-specific `RhinoSystem`/`GrasshopperSystem` paths through project properties or build arguments. Reference RhinoCommon, Grasshopper and GH_IO with `Private=false` so host assemblies are not copied into the package. If the Framework targeting pack is unavailable, the tested project used `Microsoft.NETFramework.ReferenceAssemblies` (1.0.3) only for `net48`, with `PrivateAssets=all`.

Use conditional compilation only where the runtime API differs; do not maintain two drifting panel implementations. Isolate intermediate and output paths by target framework and use a fresh build-batch output root to avoid locked binaries. An example build is `dotnet build Panel.csproj -c Release`; verify both target outputs and compiler diagnostics. Keep build scripts separate from installation so building does not unexpectedly replace the user's plugin.

## Package for runtime selection

Use a Yak package with a root manifest and runtime directories:

```text
manifest.yml
net48/ParameterPanel.rhp
net8.0-windows/ParameterPanel.rhp
```

Include each target's real non-host dependencies when needed. Package name/version/platform/minimum Rhino version must match the actual requirements; do not copy a historical package filename to imply support. Prefer the installed Yak tool's supported build route from the staged package root. If generating the ZIP directly, enforce Yak manifest/distribution rules and validate the final package with the local Yak installer and fresh-process loading. Use an explicit file allowlist to exclude old packages, build caches and synchronization metadata.

Install through Rhino's package manager/Yak within the authorized scope, verify installed hashes and let Rhino select the runtime-specific directory. A standalone `.rhp` can instead use Rhino's plugin installation/registration route; GH Libraries is not the panel's default installation destination and copying an RHP there is not GH registration. Check for stale manually registered copies when migrating to a package and preserve recoverable originals. Do not unload assemblies from the user's active session to force an update; a new installation takes effect in a fresh Rhino process.

## Verify both modes independently

Launch separate test Rhino processes with `/netfx` and `/netcore-8` for the tested baseline, rather than switching the user's global runtime preference. Use a clean document, close only test processes, and avoid modifying user parameter values during a visibility test.

For each mode, record the actual runtime version and **loaded assembly path**, load the installed plugin by its stable GUID, open the registered panel, and verify visibility and the embedded icon. Then exercise the panel's relevant behavior under that runtime: parameter discovery, edits, picking, pulse buttons, active-document changes and persistence as applicable. Separate a panel visibility smoke check from a full editing regression. Hash equality or successful compilation does not prove runtime selection or UI behavior.

The proven case was GH Parameter Panel 0.4.3 on Windows Rhino 8.20+: shared source built for both targets without warnings/errors; isolated .NET 8.0.14 and Framework processes selected their respective installed binaries and displayed the panel. That build did not revalidate every edit/pick interaction, nor did it establish Mac support. Do not transfer this compatibility result to an unrelated GHA or Python host.

Official references: [Rhino runtime migration and startup switches](https://developer.rhino3d.com/guides/rhinocommon/moving-to-dotnet-core/) and [Yak multi-target package structure](https://developer.rhino3d.com/guides/yak/the-anatomy-of-a-package/). Check them when targeting a different Rhino/runtime version.
