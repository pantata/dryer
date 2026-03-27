# Plugin Dryer para ZMOD (Klipper)

Este projeto fornece macros do Klipper para secar filamento usando a mesa aquecida da impressora.
Este plugin foi feito para a impressora Flashforge AD5X com o firmware ZMod instalado: https://github.com/ghzserg/zmod

As macros sao definidas em `dryer.cfg` e oferecem:
- inicio da secagem com base no material e no tempo,
- contagem regressiva em tempo real,
- verificacao de status,
- encerramento seguro do processo,
- notificacoes opcionais via `RESPOND PREFIX="tgalarm"`.

## Instalacao como plugin do ZMOD

Use o procedimento a seguir para instalar o plugin no ZMOD.

### 1) Adicione o plugin a `mod_data/user.moonraker.conf`

Adicione esta secao:

```ini
[update_manager dryer]
type: git_repo
channel: dev
path: /root/printer_data/config/mod_data/plugins/dryer
origin: https://github.com/pantata/dryer.git
is_system_service: False
primary_branch: master
```

- Caminho do plugin: `/root/printer_data/config/mod_data/plugins/dryer`
- Origem: `https://github.com/pantata/dryer.git`

### 2) Ativar/desativar scripts de ciclo de vida

Ativar plugin: `ENABLE_PLUGIN name=dryer`
Desativar plugin: `DISABLE_PLUGIN name=dryer`

## Instalacao alternativa (sem plugin)

Se voce nao quiser usar a instalacao baseada em plugin, pode usar uma configuracao simples com include:

1. Copie `dryer.cfg` para `mod_data` (normalmente `/root/printer_data/config/mod_data/`).
2. Adicione isto ao `user.cfg`:

```ini
[include dryer.cfg]
```

3. Reinicie o Klipper (`RESTART` ou `FIRMWARE_RESTART`).

## Uso das macros

### Iniciar secagem

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Verificar status da secagem

```gcode
DRYING_STATUS
```

### Parar secagem

```gcode
STOP_DRYING
```

## Materiais suportados e temperaturas padrao

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Se um material desconhecido for informado, a temperatura de fallback `45 °C` sera usada.

## Atalhos

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Logica de seguranca

As macros incluem protecoes basicas:
- nao iniciar se a secagem ja estiver em execucao,
- nao iniciar se a mesa ja estiver em uso,
- desligar o aquecimento automaticamente ao final,
- restaurar o valor anterior de `idle_timeout`.

## Recomendacoes

- Seque o filamento apenas sob supervisao e em um ambiente seguro.
- Verifique se a temperatura e adequada para a marca especifica do seu filamento.
- Para materiais sensiveis, comece com tempos menores e ajuste conforme necessario.
