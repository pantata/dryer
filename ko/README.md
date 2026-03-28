# ZMOD (Klipper)용 Dryer 플러그인

이 프로젝트는 프린터의 가열 베드를 사용해 필라멘트를 건조하기 위한 Klipper 매크로를 제공합니다.
이 플러그인은 ZMod 펌웨어가 설치된 Flashforge AD5X 프린터용입니다: https://github.com/ghzserg/zmod

매크로는 `dryer.cfg`에 정의되어 있으며 다음 기능을 제공합니다.
- 재료와 시간 기준 건조 시작
- 실시간 카운트다운 추적
- 상태 확인
- 안전한 종료 처리
- `RESPOND PREFIX="tgalarm"`를 통한 선택적 알림

## ZMOD 플러그인으로 설치

아래 단계에 따라 ZMOD에 플러그인을 설치하세요.

### 1) ZMOD에서 플러그인 활성화

ZMod를 업데이트하세요.
`ENABLE_EXTRA_PLUGINS` 매크로를 실행하세요.

### 2) 플러그인 활성화/비활성화

플러그인 활성화: `ENABLE_PLUGIN name=dryer`
플러그인 비활성화: `DISABLE_PLUGIN name=dryer`

## 대체 설치 방법(플러그인 없이)

플러그인 기반 설치를 원하지 않는 경우, 단순 include 설정으로 사용할 수 있습니다.

1. `dryer.cfg`를 `mod_data`로 복사하세요(일반적으로 `/root/printer_data/config/mod_data/`).
2. `user.cfg`에 다음을 추가하세요.

```ini
[include dryer.cfg]
```

3. Klipper를 재시작하세요(`RESTART` 또는 `FIRMWARE_RESTART`).

## 매크로 사용법

### 건조 시작

```gcode
START_DRYING MATERIAL=PLA TIME=4
```

### 건조 상태 확인

```gcode
DRYING_STATUS
```

### 건조 중지

```gcode
STOP_DRYING
```

## 지원 재료 및 기본 온도

- `PLA`: 45 °C
- `PETG`: 65 °C
- `TPU`: 50 °C
- `ABS`: 80 °C

알 수 없는 재료가 입력되면 기본 온도 `45 °C`가 사용됩니다.

## 바로가기

- `DRY_PLA` -> `START_DRYING MATERIAL=PLA TIME=4`
- `DRY_PETG` -> `START_DRYING MATERIAL=PETG TIME=4`
- `DRY_TPU` -> `START_DRYING MATERIAL=TPU TIME=6`
- `DRY_ABS` -> `START_DRYING MATERIAL=ABS TIME=6`

## 안전 로직

매크로에는 기본 보호 기능이 포함되어 있습니다.
- 이미 건조가 실행 중이면 시작하지 않음
- 베드가 이미 사용 중이면 시작하지 않음
- 종료 시 가열을 자동으로 끔
- 이전 `idle_timeout` 값을 복원함

## 권장 사항

- 필라멘트는 반드시 감독하에 안전한 환경에서만 건조하세요.
- 사용하는 필라멘트 브랜드에 온도가 적합한지 확인하세요.
- 민감한 재료는 더 짧은 시간부터 시작해 필요에 따라 조정하세요.
