# wylie_kicad_lib

Personal shared KiCad library: symbols, footprints, and 3D models reused across
projects. Designed to be consumed as a git submodule by projects created from
[wylie_kicad_project_template](https://github.com/TheWylieStCoyote/wylie_kicad_project_template),
but usable standalone too.

## Layout

```
symbols/wylie.kicad_sym        Shared symbols (split into wylie_power.kicad_sym etc. when it grows)
footprints/wylie.pretty/       Shared footprints (.kicad_mod)
3dmodels/wylie.3dshapes/       3D models (.step / .wrl)
```

## Conventions

- **Naming**: symbols/footprints named `<Function>_<MPN or family>` (e.g.
  `Reg_Buck_TPS62130`, `Conn_USB_C_16P`). Keep one canonical part per real-world
  component — no per-project duplicates here; one-offs belong in a project's
  local `lib/`.
- **Symbol fields**: fill `Manufacturer`, `MPN`, and `Datasheet` on every
  symbol so BOMs are complete for free.
- **3D model paths**: footprints must reference models as
  `${WYLIE_LIB_3D}/wylie.3dshapes/<model>.step` — never absolute paths. `WYLIE_LIB_3D` is a
  KiCad path variable (falls back to the environment variable of the same
  name). Template projects export it automatically from their Makefile/CI; for
  GUI work set it once in *Preferences → Configure Paths* to this repo's
  `3dmodels/` directory.

## Using outside the template

Add to a project's `sym-lib-table` / `fp-lib-table` (project-relative example,
with this repo at `libraries/wylie_kicad_lib`):

```lisp
(lib (name "wylie")(type "KiCad")(uri "${KIPRJMOD}/libraries/wylie_kicad_lib/symbols/wylie.kicad_sym")(options "")(descr "Wylie shared symbols"))
(lib (name "wylie")(type "KiCad")(uri "${KIPRJMOD}/libraries/wylie_kicad_lib/footprints/wylie.pretty")(options "")(descr "Wylie shared footprints"))
```

Target KiCad version: 10.x (file format `20251024`).
