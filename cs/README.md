# Dryer plugin pro ZMOD (Klipper)

Tento projekt obsahuje Klipper makra pro suseni filamentu pomoci vyhrivane podlozky.
Plugin je urcen pro tiskarnu Flashforge AD5X s nainstalovanym firmwarem ZMod: https://github.com/ghzserg/zmod

Makra jsou v souboru `dryer.cfg` a poskytuji:
- spusteni suseni podle materialu a casu,
- prubezne odpocitavani,
- kontrolu stavu,
- bezpecne ukonceni procesu,
- volitelne notifikace pres `RESPOND PREFIX="tgalarm"`.

## Instalace jako ZMOD plugin

Nize je spravny postup pro instalaci pluginu v ZMOD.

### 1) Pridej plugin do `mod_data/user.moonraker.conf`

Vloz sekci:

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

Povoleni pluginu: `ENABLE_PLUGIN name=dryer`
Zakazani pluginu: `DISABLE_PLUGIN name=dryer`

## Alternativni instalace (bez pluginu)

Pokud nechces instalaci pres plugin, muzes pouzit jen include konfigurace:

1. Zkopiruj `dryer.cfg` do `mod_data` (typicky do `/root/printer_data/config/mod_data/`).
2. Do `user.cfg` pridej:

```ini
[include dryer.cfg]
```

3. Proved restart Klipperu (`RESTART` nebo `FIRMWARE_RESTART`).

## Pouziti maker

### Spusteni suseni

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Stav suseni

```gcode
DRYING_STATUS
```

### Zastaveni suseni

```gcode
STOP_DRYING
```

## Podporovane materialy a vychozi teploty

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Pokud zadas neznamy material, pouzije se fallback teplota `45 °C`.

## Zkratky

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Bezpecnost logiky

Makra obsahuji zakladni ochrany:
- nespusti nove suseni, pokud uz bezi,
- nespusti suseni, pokud je podlozka uz aktivne pouzivana,
- po dokonceni automaticky vypnou topeni,
- vrati puvodni `idle_timeout`.

## Doporuceni

- Sus filament pouze pod dohledem a v bezpecnem prostredi.
- Over, ze zvolena teplota je vhodna pro konkretni znacku materialu.
- U citlivych materialu zacni kratsim casem a uprav podle vysledku.
