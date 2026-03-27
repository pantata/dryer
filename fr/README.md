# Plugin Dryer pour ZMOD (Klipper)

Ce projet fournit des macros Klipper pour secher le filament a l'aide du plateau chauffant de l'imprimante.
Ce plugin est prevu pour l'imprimante Flashforge AD5X avec le firmware ZMod installe : https://github.com/ghzserg/zmod

Les macros sont definies dans `dryer.cfg` et offrent :
- demarrage du sechage selon le materiau et la duree,
- suivi du compte a rebours en temps reel,
- consultation de l'etat,
- arret securise du processus,
- notifications optionnelles via `RESPOND PREFIX="tgalarm"`.

## Installation comme plugin ZMOD

Utilise la procedure suivante pour installer le plugin dans ZMOD.

### 1) Ajouter le plugin a `mod_data/user.moonraker.conf`

Ajoute cette section :

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: master
```

- Chemin du plugin : `/root/printer_data/config/mod_data/plugins/dryer`
- Source : `https://github.com/pantata/dryer.git`

### 2) Activer/desactiver les scripts de cycle de vie

Activer le plugin : `ENABLE_PLUGIN name=dryer`
Desactiver le plugin : `DISABLE_PLUGIN name=dryer`

## Installation alternative (sans plugin)

Si tu ne veux pas utiliser l'installation par plugin, tu peux utiliser une configuration include simple :

1. Copie `dryer.cfg` dans `mod_data` (generalement `/root/printer_data/config/mod_data/`).
2. Ajoute ceci a `user.cfg` :

```ini
[include dryer.cfg]
```

3. Redemarre Klipper (`RESTART` ou `FIRMWARE_RESTART`).

## Utilisation des macros

### Demarrer le sechage

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Verifier l'etat du sechage

```gcode
DRYING_STATUS
```

### Arreter le sechage

```gcode
STOP_DRYING
```

## Materiaux pris en charge et temperatures par defaut

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Si un materiau inconnu est fourni, la temperature de secours `45 °C` sera utilisee.

## Raccourcis

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Logique de securite

Les macros incluent des protections de base :
- ne pas demarrer si un sechage est deja en cours,
- ne pas demarrer si le plateau est deja utilise,
- couper automatiquement le chauffage a la fin,
- restaurer la valeur precedente de `idle_timeout`.

## Recommandations

- Seche le filament uniquement sous surveillance et dans un environnement sur.
- Verifie que la temperature convient a ta marque specifique de filament.
- Pour les materiaux sensibles, commence par des durees plus courtes et ajuste si necessaire.
