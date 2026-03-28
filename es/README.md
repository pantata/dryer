# Plugin Dryer para ZMOD (Klipper)

Este proyecto proporciona macros de Klipper para secar filamento usando la cama caliente de la impresora.
Este plugin esta pensado para la impresora Flashforge AD5X con el firmware ZMod instalado: https://github.com/ghzserg/zmod

Las macros estan definidas en `dryer.cfg` y ofrecen:
- inicio del secado segun material y tiempo,
- cuenta regresiva en tiempo real,
- consulta de estado,
- finalizacion segura del proceso,
- notificaciones opcionales mediante `RESPOND PREFIX="tgalarm"`.

## Instalacion como plugin de ZMOD

Sigue los pasos a continuacion para instalar el plugin en ZMOD.

### 1) Habilita el plugin en ZMOD

Actualiza ZMod.
Ejecuta la macro `ENABLE_EXTRA_PLUGINS`.

### 2) Activar/desactivar el plugin

Activar plugin: `ENABLE_PLUGIN name=dryer`
Desactivar plugin: `DISABLE_PLUGIN name=dryer`

## Instalacion alternativa (sin plugin)

Si no quieres usar instalacion basada en plugin, puedes usar una configuracion simple con include:

1. Copia `dryer.cfg` a `mod_data` (normalmente `/root/printer_data/config/mod_data/`).
2. Agrega esto a `user.cfg`:

```ini
[include dryer.cfg]
```

3. Reinicia Klipper (`RESTART` o `FIRMWARE_RESTART`).

## Uso de macros

### Iniciar secado

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Consultar estado del secado

```gcode
DRYING_STATUS
```

### Detener secado

```gcode
STOP_DRYING
```

## Materiales compatibles y temperaturas predeterminadas

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Si se proporciona un material desconocido, se usa la temperatura de respaldo `45 °C`.

## Atajos

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Logica de seguridad

Las macros incluyen protecciones basicas:
- no iniciar si el secado ya esta en ejecucion,
- no iniciar si la cama ya esta en uso,
- apagar la calefaccion automaticamente al final,
- restaurar el valor anterior de `idle_timeout`.

## Recomendaciones

- Seca el filamento solo bajo supervision y en un entorno seguro.
- Verifica que la temperatura sea adecuada para tu marca especifica de filamento.
- Para materiales sensibles, empieza con tiempos mas cortos y ajusta segun sea necesario.
