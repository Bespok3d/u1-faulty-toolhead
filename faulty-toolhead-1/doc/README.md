# Faulty Toolhead 1 Bypass

A temporary recovery aid for the Snapmaker U1. When toolhead 1's thermistor fails, the printer
raises error `0003-0523-0000-0002` and will not boot normally. This override applies Snapmaker's
documented workaround so the **other** toolheads stay usable until the faulty part is replaced.

## What it changes

For toolhead 1 (`[extruder]` + `[heater_fan e0_nozzle_fan]`):

- remaps the extruder `sensor_pin` to `e0:PC5`
- raises `max_temp` to `999` and drops `max_power` to `0.000001` (the dead sensor is ignored)
- sets the nozzle fan `heater_temp` to `999` and `fan_speed` to `0`
- ends the `stepped_temp_table` at `260, 0`

## Important

- **Do not heat or print with toolhead 1 while this is enabled.** It only exists to let you boot
  and use the healthy toolheads.
- It is independent per toolhead: if more than one is faulty, enable each affected bypass.
- Disable it again after the toolhead is repaired.

Source: Snapmaker Wiki, "Bypass a faulty toolhead".

> Not yet verified on a physical U1.
