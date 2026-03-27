# Dryer macros pro Klipper

Tento projekt obsahuje sadu Klipper macro příkazů pro sušení filamentu pomocí vyhřívané podložky tiskárny.

Konfigurace je v souboru `dryer.cfg` a přidává:
- spuštění sušení podle materiálu a času,
- průběžné odpočítávání,
- zobrazení stavu,
- bezpečné ukončení procesu,
- volitelné notifikace přes `RESPOND PREFIX="tgalarm"`.

## Co umí

- `START_DRYING MATERIAL=<typ> TIME=<hodiny>`
- Automatické nastavení teploty podle materiálu
- Timer běžící každou sekundu (`delayed_gcode DRYING_TIMER`)
- Navýšení `idle_timeout`, aby se tiskárna nevypnula během sušení
- `DRYING_STATUS` pro kontrolu zbývajícího času
- `STOP_DRYING` pro ruční nebo automatické ukončení
- Zkratkové příkazy pro běžné materiály

## Podporované materiály a výchozí teploty

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Pokud zadáš neznámý materiál, použije se fallback teplota `45 °C`.

## Rychlé použití

### 1) Přidej konfiguraci do Klipperu

Do hlavního `printer.cfg` přidej include:

```ini
[include dryer.cfg]
```

Pak proveď `RESTART` (nebo `FIRMWARE_RESTART`) v Klipperu.

### 2) Spusť sušení

Příklad pro PLA na 4 hodiny:

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### 3) Zobraz stav

```gcode
DRYING_STATUS
```

### 4) Zastav sušení

```gcode
STOP_DRYING
```

## Dostupné příkazy

- `START_DRYING MATERIAL=<PLA|PETG|TPU|ABS> TIME=<hodiny>`
  - `MATERIAL` je nepovinný (výchozí `PLA`)
  - `TIME` je nepovinný (výchozí `4` hodiny)

- `STOP_DRYING`
  - vypne podložku (`M140 S0`)
  - zastaví timer
  - vrátí původní `idle_timeout`
  - vynuluje interní proměnné

- `DRYING_STATUS`
  - ukáže zbývající čas do konce sušení

- Zkratky:
  - `DRY_PLA` → `START_DRYING MATERIAL=PLA TIME=4`
  - `DRY_PETG` → `START_DRYING MATERIAL=PETG TIME=4`
  - `DRY_TPU` → `START_DRYING MATERIAL=TPU TIME=6`
  - `DRY_ABS` → `START_DRYING MATERIAL=ABS TIME=6`

## Jak je řešená bezpečnost logiky

Macro obsahuje základní ochrany:
- nespustí nové sušení, pokud už běží,
- nespustí sušení, pokud je podložka už aktivně používaná,
- po dokončení automaticky vypne topení,
- vrací původní `idle_timeout`.

## Poznámka k notifikacím

V `dryer.cfg` jsou použité zprávy:

```gcode
RESPOND PREFIX="tgalarm" MSG="..."
```

Pokud `tgalarm` ve své instalaci nepoužíváš, můžeš tyto řádky:
- ponechat (budou jen ignorované, podle setupu), nebo
- upravit/odstranit podle vlastního notifikačního systému.

## Doporučení

- Suš filament pouze pod dohledem a v bezpečném prostředí.
- Ověř, že zvolená teplota je vhodná pro konkrétní značku materiálu.
- Pro citlivé materiály začni kratším časem a podle výsledku uprav.
