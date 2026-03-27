# Dryer plugin pro ZMOD (Klipper)

Tento projekt obsahuje Klipper makra pro sušení filamentu pomocí vyhřívané podložky.
Plugin je určen pro tiskárnu Flashforge AD5X s nainstalovaným firmwarem ZMod: https://github.com/ghzserg/zmod

Makra jsou v souboru `dryer.cfg` a poskytují:
- spuštění sušení podle materiálu a času,
- průběžné odpočítávání,
- kontrolu stavu,
- bezpečné ukončení procesu,
- volitelné notifikace přes `RESPOND PREFIX="tgalarm"`.

## Instalace jako ZMOD plugin

Níže je správný postup pro instalaci pluginu v ZMOD.

### 1) Přidej plugin do `mod_data/user.moonraker.conf`

Vlož sekci:

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: master
```

- Plugin path: `/root/printer_data/config/mod_data/plugins/dryer`
- Source: `https://github.com/pantata/dryer.git`


### 2) Enable/Disable lifecycle skripty

Povolení pluginu: `ENABLE_PLUGIN name=dryer`
Zakázání pluginu: `DISABLE_PLUGIN name=dryer`

## Alternativní instalace (bez pluginu)

Pokud nechceš instalaci přes plugin, můžeš použít jen include konfigurace:

1. Zkopíruj `dryer.cfg` do `mod_data` (typicky do `/root/printer_data/config/mod_data/`).
2. Do `user.cfg` přidej:

```ini
[include dryer.cfg]
```

3. Proveď restart Klipperu (`RESTART` nebo `FIRMWARE_RESTART`).

## Použití maker

### Spuštění sušení

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Stav sušení

```gcode
DRYING_STATUS
```

### Zastavení sušení

```gcode
STOP_DRYING
```

## Podporované materiály a výchozí teploty

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Pokud zadáš neznámý materiál, použije se fallback teplota `45 °C`.

## Zkratky

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Bezpečnost logiky

Makra obsahují základní ochrany:
- nespustí nové sušení, pokud už běží,
- nespustí sušení, pokud je podložka už aktivně používaná,
- po dokončení automaticky vypnou topení,
- vrátí původní `idle_timeout`.

## Doporučení

- Suš filament pouze pod dohledem a v bezpečném prostředí.
- Ověř, že zvolená teplota je vhodná pro konkrétní značku materiálu.
- U citlivých materiálů začni kratším časem a uprav podle výsledku.
