# Dryer-Plugin fuer ZMOD (Klipper)

Dieses Projekt enthaelt Klipper-Makros zum Trocknen von Filament mit dem beheizten Druckbett.
Dieses Plugin ist fuer den Drucker Flashforge AD5X mit installierter ZMod-Firmware vorgesehen: https://github.com/ghzserg/zmod

Die Makros sind in `dryer.cfg` definiert und bieten:
- Start des Trocknens nach Material und Zeit,
- laufende Countdown-Anzeige,
- Statusabfrage,
- sicheres Beenden des Prozesses,
- optionale Benachrichtigungen ueber `RESPOND PREFIX="tgalarm"`.

## Installation als ZMOD-Plugin

Verwende das folgende Verfahren, um das Plugin in ZMOD zu installieren.

### 1) Plugin zu `mod_data/user.moonraker.conf` hinzufuegen

Fuege diesen Abschnitt ein:

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: master
```

- Plugin-Pfad: `/root/printer_data/config/mod_data/plugins/dryer`
- Quelle: `https://github.com/pantata/dryer.git`

### 2) Lifecycle-Skripte aktivieren/deaktivieren

Plugin aktivieren: `ENABLE_PLUGIN name=dryer`
Plugin deaktivieren: `DISABLE_PLUGIN name=dryer`

## Alternative Installation (ohne Plugin)

Wenn du keine pluginbasierte Installation moechtest, kannst du eine einfache Include-Konfiguration verwenden:

1. Kopiere `dryer.cfg` nach `mod_data` (normalerweise `/root/printer_data/config/mod_data/`).
2. Fuege dies zu `user.cfg` hinzu:

```ini
[include dryer.cfg]
```

3. Starte Klipper neu (`RESTART` oder `FIRMWARE_RESTART`).

## Verwendung der Makros

### Trocknen starten

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Trocknungsstatus pruefen

```gcode
DRYING_STATUS
```

### Trocknen stoppen

```gcode
STOP_DRYING
```

## Unterstuetzte Materialien und Standardtemperaturen

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Wenn ein unbekanntes Material angegeben wird, wird die Fallback-Temperatur `45 °C` verwendet.

## Kurzbefehle

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Sicherheitslogik

Die Makros enthalten grundlegende Schutzmechanismen:
- nicht starten, wenn das Trocknen bereits laeuft,
- nicht starten, wenn das Bett bereits verwendet wird,
- Heizung am Ende automatisch ausschalten,
- vorherigen Wert von `idle_timeout` wiederherstellen.

## Empfehlungen

- Trockne Filament nur unter Aufsicht und in einer sicheren Umgebung.
- Pruefe, ob die Temperatur fuer deine konkrete Filamentmarke geeignet ist.
- Beginne bei empfindlichen Materialien mit kuerzeren Zeiten und passe sie bei Bedarf an.
