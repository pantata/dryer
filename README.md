# Dryer Plugin for ZMOD (Klipper)

This project provides Klipper macros for drying filament using the printer's heated bed.
This plugin is intended for the Flashforge AD5X printer with ZMod firmware installed: https://github.com/ghzserg/zmod

Macros are defined in `dryer.cfg` and provide:
- material/time-based drying start,
- live countdown tracking,
- status reporting,
- safe process shutdown,
- optional notifications via `RESPOND PREFIX="tgalarm"`.

## Installation as a ZMOD Plugin

Follow the steps below to install the plugin in ZMOD.

### 1) Enable the plugin in ZMOD

Update ZMod.
Run the `ENABLE_EXTRA_PLUGINS` macro.

### 2) Enable/Disable the plugin

Enable plugin: `ENABLE_PLUGIN name=dryer`
Disable plugin: `DISABLE_PLUGIN name=dryer`

## Alternative Installation (without plugin)

If you do not want plugin-based installation, you can use a plain include setup:

1. Copy `dryer.cfg` to `mod_data` (typically `/root/printer_data/config/mod_data/`).
2. Add this to `user.cfg`:

```ini
[include dryer.cfg]
```

3. Restart Klipper (`RESTART` or `FIRMWARE_RESTART`).

## Macro Usage

### Start drying

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Check drying status

```gcode
DRYING_STATUS
```

### Stop drying

```gcode
STOP_DRYING
```

## Supported Materials and Default Temperatures

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

If an unknown material is provided, the fallback temperature is `45 °C`.

## Shortcuts

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

## Recommendations

- Dry filament only under supervision and in a safe environment.
- Verify temperature suitability for your specific filament brand.
- For sensitive materials, start with shorter times and adjust as needed.
