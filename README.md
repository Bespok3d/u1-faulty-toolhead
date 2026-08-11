# u1-faulty-toolhead

[![licence](https://img.shields.io/badge/licence-GPL--3.0-blue)](LICENSE)
[![release](https://img.shields.io/github/v/release/Bespok3d/u1-faulty-toolhead)](https://github.com/Bespok3d/u1-faulty-toolhead/releases)
![printer](https://img.shields.io/badge/printer-Snapmaker%20U1-informational)
![stock firmware](https://img.shields.io/badge/stock%20firmware-no%20flashing-brightgreen)

A co-repo of recovery Bespok3d plugins for the Snapmaker U1. When a toolhead's thermistor fails the
printer raises error `0003-0523-0000-0002` and will not boot. Each plugin here drops a single Klipper
config override that ignores the dead sensor for one toolhead, so the remaining toolheads stay usable
until the part is replaced. Ported from the extended firmware's `34-feature-faulty-toolhead`.

- `faulty-toolhead-1` .. `faulty-toolhead-4`: one per toolhead, independent (enable each affected one).

Each override remaps the extruder `sensor_pin` to `PC5`, neutralizes the heater (`max_power` ~ 0,
`max_temp` 999) and the matching nozzle fan, and is a temporary aid only: do not heat or print with
the faulty toolhead while it is enabled, and remove it after repair.

No scripts, no patches, no binaries. Config-include plugins; Klipper restarts on install.

> Installed and running on a Snapmaker U1.

## Build locally

Needs Node.js 20+. Builds run through the shared `Bespok3d/b3-builder` tool:

```sh
npm install github:Bespok3d/b3-builder
npx b3-builder build --source ./faulty-toolhead-1 --atom-repo Bespok3d/u1-faulty-toolhead
# -> dist/faulty-toolhead-1-<ver>.b3 + dist/faulty-toolhead-1.atom.json
```

Drop `--source` to build every plugin in the repo at once.

Writing a plugin of your own? Start at the plugin documentation:
[Bespok3d/b3-builder/doc](https://github.com/Bespok3d/b3-builder/tree/main/doc).

## Releasing

Bump a plugin's `manifest.json` `version` and push the tag `plugin-<name>-v<version>` naming that
plugin and that exact number. A push to `main` publishes nothing, and the run is refused if the tag
and the manifest disagree. CI runs the `Bespok3d/b3-builder` Action over the whole repo, which packs
each `.b3`, cuts a release per plugin, assembles this repo's `index.json` sub-list as `U1 Faulty
Toolhead Bypass`, and registers it in `Bespok3d/main-index` (`lists/<repo>.json`). Secrets:
`MAIN_INDEX_TOKEN` (contents:write on main-index) and `REGISTRY_SIGNING_KEY` (the org registry key
the `b3-builder` Action signs each `.b3` and atom with).

## Maintainership

These plugins are published and maintained by the Bespok3d org, and several of them repackage or
build on upstream source material. If you own the source material a plugin is based on and would
rather manage it yourself, you are welcome to contact the org to claim it back. The one condition is
that it stays actively maintained: a claimed plugin left to rot will be reclaimed so users are never
stranded on an abandoned package.

## Licence

Copyright (C) 2026 unlucio and the Bespok3d contributors

This repo ships code from other projects offered under version 3 of the GNU General Public License,
with no option to use a later version, so version 3 of that licence covers every file in this repo.

This program is free software: you can redistribute it and/or modify it under the terms of version 3
of the GNU General Public License as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without
even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not,
see <https://www.gnu.org/licenses/>. The full text is in [LICENSE](LICENSE).

Bespok3d's own code elsewhere is AGPL-3.0-or-later. One licence covering this whole repo is a clarity
choice, so that nobody has to work out which file carries which terms. Version 3 of the GPL and
version 3 of the AGPL may be combined in a single work, and section 13 of each licence says so; what
cannot happen is code offered under version 3 of the GPL alone being re-offered under the AGPL.

Bespok3d is a project of the Bespok3d Organisation, which is not a legal entity. Copyright is held by
the individual authors named above.
