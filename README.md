# 3MF Viewer – Total Commander Lister Plugin (WLX64)

Interactive OpenGL viewer for `.3mf` (3D Manufacturing Format) and `stl` files.

## Features

| Control | Action |
|---------|--------|
| Left drag | Rotate model |
| Right drag | Pan |
| Mouse wheel | Zoom |
| Double-click | Reset view |
| `R` / `F5` | Reset view |
| `W` | Cycle: Solid → Solid+Wire → Wireframe |
| Arrow keys | Rotate in 5° steps |
| `+` / `-` | Zoom in / out |

TC dark mode is detected automatically via `lcp_darkmode`.

---

## Project structure

```
3mfviewer/
├── 3mfviewer.lpi          Lazarus project (open this in the IDE)
├── 3mfviewer.lpr          Library source / exports
├── plugin_main.pas        WLX export functions + form lifecycle
├── viewer_form.pas        TViewerForm – TOpenGLControl + rendering
├── mesh_data.pas          lib3mf loading + vertex normal computation
├── listplug.pas           WLX SDK header (v2.12)
└── lib3mf/
    └── Unit_Lib3MF.pas    Pascal bindings (from lib3mf-2.5.0-Windows.zip)
```

---

## Prerequisites

1. **Lazarus 3.x** with **FPC 3.2.x** — 64-bit (win64) toolchain installed  
   Download: https://www.lazarus-ide.org/

2. **`OpenGLContext` package** — ships with Lazarus; install via  
   `Package → Install/Uninstall Packages → OpenGLContext → Install`

3. **`lib3mf.dll`** (from the lib3mf-2.5.0-Windows release) must be placed  
   in the **same directory** as `3mfviewer.wlx64`.

---

## Build steps

### In the Lazarus IDE

1. `File → Open Project` → select `3mfviewer.lpi`
2. Verify the active build mode is **Release** (top toolbar)
3. `Run → Build` (or `Shift+F9`)
4. Output: `3mfviewer.wlx64` in the project directory

### Command line

```bat
lazbuild 3mfviewer.lpi --build-mode=Release
```

---

## Installation in Total Commander

1. Copy these files to a single directory, e.g. `%COMMANDER_PATH%\Plugins\Lister\3mfviewer\`:
   - `3mfviewer.wlx64`
   - `lib3mf.dll`

2. In TC: `Configuration → Options → Plugins → Lister Plugins → Add`  
   Select `3mfviewer.wlx64`.

3. TC will store the detect string `EXT="3MF" and EXT="STL"` in `wincmd.ini` automatically.  
   After that, pressing `F3` on any `.3mf` file opens the plugin.

---

## Notes

- The plugin is **64-bit only** (`.wlx64` extension). It requires TC 64-bit.  
  For TC 32-bit, recompile with target `i386-win32` and output extension `.wlx`.

- All mesh objects in the 3MF file are merged into one draw call.  
  Build-item transforms are not yet applied (raw model-space geometry).

- Smooth vertex normals are computed by averaging adjacent face normals.

- OpenGL 1.5+ is required (universally available on any Windows 7+ system).
