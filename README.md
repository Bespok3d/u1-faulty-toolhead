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
