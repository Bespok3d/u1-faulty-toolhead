# u1-faulty-toolhead

A co-repo of recovery Bespok3d plugins for the Snapmaker U1. When a toolhead's thermistor fails the
printer raises error `0003-0523-0000-0002` and will not boot. Each plugin here drops a single Klipper
config override that ignores the dead sensor for one toolhead, so the remaining toolheads stay usable
until the part is replaced. Ported from the extended firmware's `34-feature-faulty-toolhead`.

- `faulty-toolhead-1` .. `faulty-toolhead-4`: one per toolhead, independent (enable each affected one).

Each override remaps the extruder `sensor_pin` to `PC5`, neutralizes the heater (`max_power` ~ 0,
`max_temp` 999) and the matching nozzle fan, and is a temporary aid only: do not heat or print with
the faulty toolhead while it is enabled, and remove it after repair.

No scripts, no patches, no binaries. Config-include plugins; Klipper restarts on install.

> Not yet verified on a physical U1.

## Build locally

Needs Node.js 20+. Builds run through the shared `Bespok3d/b3-builder` tool:

```sh
npm install github:Bespok3d/b3-builder
npx b3-builder build --source ./faulty-toolhead-1 --atom-repo Bespok3d/u1-faulty-toolhead
# -> dist/faulty-toolhead-1-<ver>.b3 + dist/faulty-toolhead-1.atom.json
```

Drop `--source` to build every plugin in the repo at once.

## Releasing

Bump a plugin's `manifest.json` `version` and push to `main`. CI runs the `Bespok3d/b3-builder`
Action over the whole repo, which packs each `.b3`, cuts a release per plugin, assembles this repo's
`index.json` sub-list as `U1 Faulty Toolhead Bypass`, and registers it in `Bespok3d/main-index`
(`lists/<repo>.json`). Secrets: `MAIN_INDEX_TOKEN` (contents:write on main-index) and
`REGISTRY_SIGNING_KEY` (the org registry key the `b3-builder` Action signs each `.b3` and atom with).

## Maintainership

These plugins are published and maintained by the Bespok3d org, and several of them repackage or
build on upstream source material. If you own the source material a plugin is based on and would
rather manage it yourself, you are welcome to contact the org to claim it back. The one condition is
that it stays actively maintained: a claimed plugin left to rot will be reclaimed so users are never
stranded on an abandoned package.
