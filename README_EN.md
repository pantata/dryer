# Dryer Macros for Klipper

This project provides a set of Klipper macros for drying filament using the printer's heated bed.

The configuration is stored in `dryer.cfg` and adds:
- material/time-based drying start,
- live countdown tracking,
- drying status output,
- safe process shutdown,
- optional notifications via `RESPOND PREFIX="tgalarm"`.

## Features

- `START_DRYING MATERIAL=<type> TIME=<hours>`
- Automatic temperature selection based on material
- A 1-second timer loop (`delayed_gcode DRYING_TIMER`)
- Temporary `idle_timeout` extension to prevent shutdown during drying
- `DRYING_STATUS` to check remaining time
- `STOP_DRYING` for manual or automatic stop
- Shortcut macros for common materials

## Supported Materials and Default Temperatures

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

If an unknown material is provided, the fallback temperature is `45 °C`.

## Quick Start

### 1) Include the config in Klipper

Add this line to your main `printer.cfg`:

```ini
[include dryer.cfg]
```

Then run `RESTART` (or `FIRMWARE_RESTART`) in Klipper.

### 2) Start drying

Example: PLA for 4 hours

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### 3) Check status

```gcode
DRYING_STATUS
```

### 4) Stop drying

```gcode
STOP_DRYING
```

## Available Commands

- `START_DRYING MATERIAL=<PLA|PETG|TPU|ABS> TIME=<hours>`
  - `MATERIAL` is optional (default: `PLA`)
  - `TIME` is optional (default: `4` hours)

- `STOP_DRYING`
  - turns off the bed (`M140 S0`)
  - stops the timer
  - restores the original `idle_timeout`
  - resets internal macro variables

- `DRYING_STATUS`
  - displays remaining drying time

- Shortcuts:
  - `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
  - `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
  - `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
  - `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Safety Logic

The macros include basic protection checks:
- do not start if drying is already running,
- do not start if the bed is already in use,
- automatically turn heating off at the end,
- restore the previous `idle_timeout` value.

## Notification Note

`dryer.cfg` includes messages such as:

```gcode
RESPOND PREFIX="tgalarm" MSG="..."
```

If your setup does not use `tgalarm`, you can:
- keep these lines (they may be ignored depending on your setup), or
- modify/remove them to match your notification system.

## Recommendations

- Dry filament only under supervision and in a safe environment.
- Verify temperature suitability for your specific filament brand.
- For sensitive materials, start with shorter times and adjust as needed.
