# ZMOD (Klipper) icin Dryer Eklentisi

Bu proje, yazicinin isitilan tablasini kullanarak filament kurutmak icin Klipper makrolari saglar.
Bu eklenti, ZMod firmware'i yuklu olan Flashforge AD5X yazicisi icin tasarlanmistir: https://github.com/ghzserg/zmod

Makrolar `dryer.cfg` dosyasinda tanimlanmistir ve sunlari saglar:
- malzeme ve sureye gore kurutma baslatma,
- canli geri sayim takibi,
- durum raporlama,
- islemin guvenli sekilde sonlandirilmasi,
- `RESPOND PREFIX="tgalarm"` ile istege bagli bildirimler.

## ZMOD eklentisi olarak kurulum

Eklentiyi ZMOD'a kurmak icin asagidaki adimlari izleyin.

### 1) Eklentiyi ZMOD icinde etkinlestirin

ZMod'u guncelleyin.
`ENABLE_EXTRA_PLUGINS` makrosunu calistirin.

### 2) Eklentiyi etkinlestirme/devre disi birakma

Eklentiyi etkinlestir: `ENABLE_PLUGIN name=dryer`
Eklentiyi devre disi birak: `DISABLE_PLUGIN name=dryer`

## Alternatif kurulum (eklenti olmadan)

Eklenti tabanli kurulum istemiyorsaniz, basit bir include kurulumu kullanabilirsiniz:

1. `dryer.cfg` dosyasini `mod_data` klasorune kopyalayin (genellikle `/root/printer_data/config/mod_data/`).
2. `user.cfg` icine sunu ekleyin:

```ini
[include dryer.cfg]
```

3. Klipper'i yeniden baslatin (`RESTART` veya `FIRMWARE_RESTART`).

## Makro kullanimi

### Kurutmayi baslat

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### Kurutma durumunu kontrol et

```gcode
DRYING_STATUS
```

### Kurutmayi durdur

```gcode
STOP_DRYING
```

## Desteklenen malzemeler ve varsayilan sicakliklar

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

Bilinmeyen bir malzeme girilirse, `45 °C` yedek sicakligi kullanilir.

## Kisayollar

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## Guvenlik mantigi

Makrolar temel koruma kontrolleri icerir:
- kurutma zaten calisiyorsa baslatma,
- tabla zaten kullaniliyorsa baslatma,
- sonunda isitmayi otomatik kapatma,
- onceki `idle_timeout` degerini geri yukleme.

## Oneriler

- Filamenti yalnizca gozetim altinda ve guvenli bir ortamda kurutun.
- Sicakligin kullandiginiz filament markasi icin uygun oldugunu dogrulayin.
- Hassas malzemelerde daha kisa surelerle baslayin ve gerektikce ayarlayin.
