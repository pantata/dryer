# Dryer Plugin for ZMOD (Klipper)

This project provides Klipper macros for drying filament using the printer's heated bed.

Macros are defined in `dryer.cfg` and provide:
- material/time-based drying start,
- live countdown tracking,
- status reporting,
- safe process shutdown,
- optional notifications via `RESPOND PREFIX="tgalarm"`.

## Installation as a ZMOD Plugin

Use the following procedure to install the plugin in ZMOD.

### 1) Add the plugin to `mod_data/user.moonraker.conf`

Add this section:

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: main
```

- Plugin path: `/root/printer_data/config/mod_data/plugins/dryer`
- Source: `https://github.com/pantata/dryer.git`

### 2) Enable/Disable lifecycle scripts

Enable plugin: `ENABLE_PLUGIN name=dryer`
Disable plugin: `DISABLE_PLUGIN name=dryer`

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
