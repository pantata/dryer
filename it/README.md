# Plugin Dryer per ZMOD (Klipper)

Questo progetto fornisce macro Klipper per asciugare il filamento usando il piano riscaldato della stampante.
Questo plugin e pensato per la stampante Flashforge AD5X con firmware ZMod installato: https://github.com/ghzserg/zmod

Le macro sono definite in `dryer.cfg` e offrono:
- avvio dell'asciugatura in base a materiale e tempo,
- conto alla rovescia in tempo reale,
- controllo dello stato,
- arresto sicuro del processo,
- notifiche opzionali tramite `RESPOND PREFIX="tgalarm"`.

## Installazione come plugin ZMOD

Usa la seguente procedura per installare il plugin in ZMOD.

### 1) Aggiungi il plugin a `mod_data/user.moonraker.conf`

Aggiungi questa sezione:

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: master
```

- Percorso plugin: `/root/printer_data/config/mod_data/plugins/dryer`
- Sorgente: `https://github.com/pantata/dryer.git`

### 2) Abilitare/disabilitare gli script di ciclo di vita

Abilita plugin: `ENABLE_PLUGIN name=dryer`
Disabilita plugin: `DISABLE_PLUGIN name=dryer`

## Installazione alternativa (senza plugin)

Se non vuoi usare l'installazione tramite plugin, puoi usare una semplice configurazione include:

1. Copia `dryer.cfg` in `mod_data` (di solito `/root/printer_data/config/mod_data/`).
2. Aggiungi questo a `user.cfg`:

```ini
[include dryer.cfg]
```

3. Riavvia Klipper (`RESTART` oppure `FIRMWARE_RESTART`).

## Uso delle macro

### Avviare l'asciugatura

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Controllare lo stato dell'asciugatura

```gcode
DRYING_STATUS
```

### Fermare l'asciugatura

```gcode
STOP_DRYING
```

## Materiali supportati e temperature predefinite

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Se viene fornito un materiale sconosciuto, viene usata la temperatura di fallback `45 °C`.

## Scorciatoie

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Logica di sicurezza

Le macro includono protezioni di base:
- non avviare se l'asciugatura e gia in corso,
- non avviare se il piano e gia in uso,
- spegnere automaticamente il riscaldamento alla fine,
- ripristinare il valore precedente di `idle_timeout`.

## Raccomandazioni

- Asciuga il filamento solo sotto supervisione e in un ambiente sicuro.
- Verifica che la temperatura sia adatta al tuo specifico marchio di filamento.
- Per i materiali sensibili, inizia con tempi piu brevi e regola se necessario.
