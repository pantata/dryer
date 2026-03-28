# ZMOD (Klipper) 用 Dryer プラグイン

このプロジェクトは、プリンターのヒートベッドを使ってフィラメントを乾燥させるための Klipper マクロを提供します。
このプラグインは、ZMod ファームウェアがインストールされた Flashforge AD5X プリンター向けです: https://github.com/ghzserg/zmod

マクロは `dryer.cfg` に定義されており、次の機能を提供します。
- 材料と時間を指定した乾燥開始
- リアルタイムのカウントダウン表示
- 状態確認
- 安全な停止処理
- `RESPOND PREFIX="tgalarm"` による任意の通知

## ZMOD プラグインとしてインストール

ZMOD にプラグインをインストールするには、以下の手順に従ってください。

### 1) ZMOD でプラグインを有効化

ZMod を更新します。
`ENABLE_EXTRA_PLUGINS` マクロを実行します。

### 2) プラグインの有効化/無効化

プラグインを有効化: `ENABLE_PLUGIN name=dryer`
プラグインを無効化: `DISABLE_PLUGIN name=dryer`

## 代替インストール方法（プラグインなし）

プラグイン方式を使いたくない場合は、単純な include 設定でも使用できます。

1. `dryer.cfg` を `mod_data` にコピーします（通常は `/root/printer_data/config/mod_data/`）。
2. `user.cfg` に次を追加します。

```ini
[include dryer.cfg]
```

3. Klipper を再起動します（`RESTART` または `FIRMWARE_RESTART`）。

## マクロの使い方

### 乾燥を開始

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### 乾燥状態を確認

```gcode
DRYING_STATUS
```

### 乾燥を停止

```gcode
STOP_DRYING
```

## 対応材料とデフォルト温度

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

不明な材料が指定された場合は、フォールバック温度 `45 °C` が使用されます。

## ショートカット

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## 安全ロジック

マクロには基本的な保護機能が含まれています。
- 乾燥中の場合は開始しない
- ベッドがすでに使用中の場合は開始しない
- 終了時に加熱を自動でオフにする
- 以前の `idle_timeout` の値を復元する

## 推奨事項

- フィラメントの乾燥は必ず監視下で安全な環境で行ってください。
- 使用するフィラメントのブランドに温度が適しているか確認してください。
- デリケートな材料では、短い時間から始めて必要に応じて調整してください。
