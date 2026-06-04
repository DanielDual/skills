---
name: how-to-use-gopeak-godot-mcp
description: "Information for using `gopeak-godot-mcp`, the MCP server for Godot that lets AI assistants run, inspect, modify, and debug real projects end-to-end.
Read this if you want to do godot-specific actions."
---

## Introduction

**GoPeak is an MCP server for Godot that lets AI assistants run, inspect, modify, and debug real projects end-to-end.**

## Core Capabilities

- **Project control**: launch editor, run/stop project, capture debug output
- **Scene editing**: create scenes, add/delete/reparent nodes, edit properties
- **Script workflows**: create/modify scripts, inspect script structure
- **Resources**: create/modify resources, materials, shaders, tilesets
- **Signals/animation**: connect signals, build animations/tracks/state machines
- **Runtime tools**: inspect live tree, set properties, call methods, metrics
- **LSP + DAP**: diagnostics/completion/hover + breakpoints/step/stack trace
- **Input + screenshots**: keyboard/mouse/action injection and viewport capture
- **Asset library**: search/fetch CC0 assets (Poly Haven, AmbientCG, Kenney)

## Server Configuration
In this project, `gopeak-godot-mcp` is configured in the way below:

### Environment variables

| Name                     | Purpose                                                                                    | Value       |
| ------------------------ | ------------------------------------------------------------------------------------------ | ----------- |
| `GOPEAK_TOOL_PROFILE`    | Tool exposure profile: `compact`, `full`, `legacy`                                         | `compact`   |
| `MCP_TOOL_PROFILE`       | Fallback profile env alias                                                                 | `compact`   |
| `GODOT_PATH`             | Explicit Godot executable path                                                             | `godot`     |
| `GODOT_BRIDGE_PORT`      | Bridge/Visualizer HTTP+WS port override (aliases: `MCP_BRIDGE_PORT`, `GOPEAK_BRIDGE_PORT`) | `6505`      |
| `DEBUG`                  | Enable server debug logs (`true`/`false`)                                                  | `false`     |
| `LOG_MODE`               | Recording mode: `lite` or `full`                                                           | `lite`      |
| `GOPEAK_TOOLS_PAGE_SIZE` | Number of tools per `tools/list` page (pagination)                                         | `33`        |
| `GOPEAK_BRIDGE_PORT`     | Port for unified bridge/visualizer server                                                  | `6505`      |
| `GOPEAK_BRIDGE_HOST`     | Bind host for bridge/visualizer server                                                     | `127.0.0.1` |

### Ports

| Port   | Service                                                                               |
| ------ | ------------------------------------------------------------------------------------- |
| `6505` | Unified Godot Bridge + Visualizer server (+ `/health`, `/mcp`) on loopback by default |
| `6005` | Godot LSP                                                                             |
| `6006` | Godot DAP                                                                             |
| `7777` | Runtime addon command socket (only needed for runtime tools)                          |
## Dynamic Tool Groups (compact mode)

In `compact` mode, 78 additional tools are organized into **22 groups** that activate automatically when needed:

|Group|Tools|Description|
|---|---|---|
|`scene_advanced`|3|Duplicate, reparent nodes, load sprites|
|`uid`|2|UID management for resources|
|`import_export`|5|Import pipeline, reimport, validate project|
|`autoload`|4|Autoload singletons, main scene|
|`signal`|2|Disconnect signals, list connections|
|`runtime`|4|Live scene inspection, runtime properties, metrics|
|`resource`|4|Create/modify materials, shaders, resources|
|`animation`|5|Animations, tracks, animation tree, state machine|
|`plugin`|3|Enable/disable/list editor plugins|
|`input`|1|Input action mapping|
|`tilemap`|2|TileSet and TileMap cell painting|
|`audio`|4|Audio buses, effects, volume|
|`navigation`|2|Navigation regions and agents|
|`theme_ui`|3|Theme colors, font sizes, shaders|
|`asset_store`|3|Search/download CC0 assets|
|`testing`|6|Screenshots, viewport capture, input injection|
|`dx_tools`|4|Error log, project health, find usages, scaffold|
|`intent_tracking`|9|Intent capture, decision logs, handoff briefs|
|`class_advanced`|1|Class inheritance inspection|
|`lsp`|3|GDScript completions, hover, symbols|
|`dap`|6|Breakpoints, stepping, stack traces|
|`version_gate`|2|Version validation, patch verification|

**How it works:**

1. **Auto-activation via catalog**: Search with `tool.catalog` and matching groups activate automatically.
    
    > "Use `tool.catalog` with query `animation` and show relevant tools."
    
2. **Manual activation**: Directly activate a group with `tool.groups`.
    
    > "Use `tool.groups` to activate the `dap` group for debugging."
    
3. **Deactivation**: Remove groups when done to reduce context.
    
    > "Use `tool.groups` to reset all active groups."