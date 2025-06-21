# ⚙️ 설정 가이드

DonationAPI의 모든 설정 파일과 옵션을 상세히 설명합니다.

## 📋 목차

- [기본 설정](#-기본-설정)
- [이벤트 설정 파일](#-이벤트-설정-파일)
- [연동 데이터 파일](#-연동-데이터-파일)
- [고급 설정](#-고급-설정)
- [설정 파일 관리](#-설정-파일-관리)

## 🔧 기본 설정

### config.yml - 메인 설정 파일

플러그인의 핵심 설정을 관리하는 파일입니다.

#### 파일 위치
```
plugins/DonationAPI/config.yml
```

#### 전체 설정 구조
```yaml
# API 연결 설정
api:
  key: "YOUR_API_KEY_HERE"           # 발급받은 API 키
  url: "http://121.145.190.195:3000" # API 서버 URL (변경 금지)

# Socket.IO 연결 설정
socket:
  url: "http://121.145.190.195:3001" # Socket.IO 서버 URL (변경 금지)
  reconnect-interval: 5              # 재연결 간격 (초)

# 이벤트 처리 설정
events:
  async: true          # 비동기 이벤트 처리
  execution-delay: 1   # 이벤트 실행 지연 (초)
  event-type: "amount" # "amount" (금액) 또는 "count" (개수)

# 디버그 설정
debug:
  enabled: false # 디버그 모드 활성화
  detail: false  # 상세 디버그 로그

# 미션 시스템 설정 (숲 전용)
mission:
  enabled: true # 미션 시스템 활성화
  
  # 정확한 매칭 설정
  exact_match:
    only: false     # 정확한 금액만 허용 (오차 범위 무시)
    priority: true  # 정확한 금액 우선 실행
  
  # 하향 오차 범위 (부족한 금액)
  down_tolerance:
    enabled: false        # 하향 오차 범위 활성화
    type: "percent"       # "percent" 또는 "amount"
    percent: 5.0          # 퍼센트 오차 (5%)
    amount: 100           # 고정 금액 오차 (100원)
    ratio_execution: false # 비율 실행 (배수만큼 반복)
  
  # 상향 오차 범위 (초과한 금액)
  up_tolerance:
    enabled: false         # 상향 오차 범위 활성화
    type: "percent"        # "percent" 또는 "amount"
    percent: 5.0           # 퍼센트 오차 (5%)
    amount: 100            # 고정 금액 오차 (100원)
    multiple_execution: false # 다중 실행
    max_events: 3          # 최대 이벤트 실행 수
  
  # 검색 설정
  search:
    direction: "both"      # "up", "down", "both"
    closest_first: true    # 가장 가까운 것 우선
    remove_duplicates: true # 중복 제거
  
  # 로깅 설정
  logging:
    enabled: true  # 미션 로깅
    detail: false  # 상세 로깅

# Velocity 네트워크 설정
velocity:
  enabled: false # Velocity 모드 활성화

# 메시지 설정
messages:
  prefix: "&8[&6DonationAPI&8]&f " # 메시지 접두사
  
  # 성공 메시지
  success:
    stream-connected: "&a방송이 성공적으로 연동되었습니다!"
    event-registered: "&a이벤트가 등록되었습니다!"
    listener-started: "&a후원 리스너가 시작되었습니다."
  
  # 오류 메시지
  error:
    already-connected: "&c이미 연동된 방송이 있습니다."
    invalid-platform: "&c올바른 플랫폼을 입력해주세요. (치지직/숲)"
    invalid-amount: "&c올바른 금액을 입력해주세요."
    no-permission: "&c권한이 없습니다."
    
  # 관리자 메시지
  admin:
    reload-success: "&a플러그인이 성공적으로 리로드되었습니다."
    test-executed: "&a테스트 후원이 실행되었습니다."
```

### 설정 항목 상세 설명

#### API 설정
```yaml
api:
  key: "YOUR_API_KEY_HERE"  # 필수! 디스코드에서 발급받은 API 키
  url: "http://121.145.190.195:3000"  # 변경하지 마세요
```

#### 이벤트 타입 설정
```yaml
events:
  event-type: "amount"  # 금액 기준 이벤트
  # 또는
  event-type: "count"   # 개수 기준 이벤트
```

**차이점:**
- `amount`: 후원 금액을 기준으로 이벤트 실행 (1000원, 5000원...)
- `count`: 후원 개수를 기준으로 이벤트 실행 (1개, 5개...)

#### 미션 시스템 세부 설정

**정확한 매칭**
```yaml
exact_match:
  only: true    # 정확한 금액만 허용, 오차 범위 완전 무시
  priority: true # 정확한 금액이 있으면 오차 범위보다 우선
```

**오차 범위 타입**
```yaml
down_tolerance:
  type: "percent"  # 퍼센트 기준 (5% = 1000원의 5% = 50원)
  percent: 5.0
  
  # 또는
  type: "amount"   # 고정 금액 기준 (100원 고정)
  amount: 100
```

## 📁 이벤트 설정 파일

이벤트 설정은 YAML 파일로 저장되며, 각 대상별로 별도 파일로 관리됩니다.

### 파일 구조
```
plugins/DonationAPI/events/
├── common.yml          # 공통 이벤트
├── platforms/          # 플랫폼별 이벤트 폴더
│   ├── chzzk.yml      # 치지직 플랫폼 이벤트
│   └── soop.yml       # 숲 플랫폼 이벤트
└── users/             # 개별 스트리머 이벤트 폴더
    └── [UUID].yml     # 개별 스트리머 이벤트
```

### common.yml - 공통 이벤트 설정

모든 스트리머에게 적용되는 이벤트를 설정합니다.

```yaml
# 1000원 이벤트 설정
1000:
  # 전체 대상 이벤트
  ALL:
    actions:
      # 메시지 액션
      - type: MESSAGE
        messages:
          chat:
            - "&a%donator_name%님 %amount%원 후원 감사합니다!"
          title:
            - "&6감사합니다!"
          subtitle:
            - "&e%donator_name%님"
          actionbar:
            - "&c후원: %amount%원"
      
      # 명령어 액션
      - type: COMMAND
        command: "CONSOLE:/say %donator_name%님이 %amount%원을 후원하셨습니다!"
  
  # 스트리머 대상 이벤트
  STREAMER:
    actions:
      # 아이템 액션
      - type: GIVE_ITEM
        items: "H4sIAAAAAAAAAD1Ru07DQBC8Nxf..." # Base64 인코딩된 아이템 데이터
      
      # 명령어 액션
      - type: COMMAND
        command: "PLAYER:/heal"

# 범위 이벤트 (1000~5000원)
-1000005000:  # 범위는 음수로 인코딩됨
  ALL:
    actions:
      - type: MESSAGE
        messages:
          chat:
            - "&a%donator_name%님 범위 후원 감사합니다!"

# 무한 이벤트 (모든 금액)
-1:  # 무한은 -1로 표시
  ALL:
    actions:
      - type: MESSAGE
        messages:
          chat:
            - "&7%donator_name%님 후원 감사드립니다!"

# 애드온 이벤트
5000:
  STREAMER:
    actions:
      - type: ADDON
        addon: "launch_to_sky"
        variables:
          power: "5"
          duration: "10"
```

### 개별 스트리머 이벤트 파일

UUID를 파일명으로 하는 개별 스트리머 설정 파일입니다.

**파일명 예시:** `a1b2c3d4-e5f6-7890-abcd-ef1234567890.yml`

```yaml
# steve 플레이어의 개별 이벤트
1000:
  STREAMER:
    actions:
      - type: MESSAGE
        messages:
          title:
            - "&cSteve 전용 이벤트!"
      - type: COMMAND
        command: "CONSOLE:/give steve diamond 5"

# steve만의 특별 범위 이벤트
-5000010000:  # 5000~10000원
  ALL:
    actions:
      - type: MESSAGE
        messages:
          chat:
            - "&dSteve님의 특별 이벤트! %amount%원!"
```

### platforms/chzzk.yml / platforms/soop.yml - 플랫폼별 이벤트

특정 플랫폼에서만 실행되는 이벤트를 설정합니다.

**platforms/chzzk.yml (치지직 전용)**
```yaml
1000:
  ALL:
    actions:
      - type: MESSAGE
        messages:
          chat:
            - "&9[치지직] &a%donator_name%님 치즈 후원 감사합니다!"
```

**platforms/soop.yml (숲 전용)**
```yaml
1000:
  ALL:
    actions:
      - type: MESSAGE
        messages:
          chat:
            - "&6[숲] &a%donator_name%님 별풍선 후원 감사합니다!"

# 도전미션 전용 이벤트
1234:
  ALL:
    actions:
      - type: MESSAGE
        messages:
          title:
            - "&6🎯 도전미션 성공!"
          subtitle:
            - "&e%donator_name%님 정확히 맞추셨습니다!"
```

### 액션 타입별 데이터 구조

#### MESSAGE 액션
```yaml
- type: MESSAGE
  messages:
    chat:              # 채팅 메시지 (배열)
      - "첫 번째 채팅 메시지"
      - "두 번째 채팅 메시지"
    title:             # 타이틀 메시지 (배열)
      - "타이틀 텍스트"
    subtitle:          # 서브타이틀 메시지 (배열)
      - "서브타이틀 텍스트"
    actionbar:         # 액션바 메시지 (배열)
      - "액션바 텍스트"
```

#### COMMAND 액션
```yaml
- type: COMMAND
  command: "CONSOLE:/give %player% diamond 1"
  # 또는
  command: "PLAYER:/heal"
  # 또는 멀티라인
  command: "CONSOLE:/give %player% diamond 1\nPLAYER:/heal"
```

#### GIVE_ITEM 액션
```yaml
- type: GIVE_ITEM
  items: "H4sIAAAAAAAAAD1Ru07DQBC8Nxf..."  # Base64 인코딩된 아이템 데이터
```

#### ADDON 액션
```yaml
- type: ADDON
  addon: "launch_to_sky"    # 애드온 이름
  variables:                # 전달할 변수들
    power: "5"
    duration: "10"
    effect: "fire"
```

## 💾 연동 데이터 파일

### linkdata.yml - 방송 연동 정보

연동된 스트리머 정보를 저장하는 파일입니다.

```yaml
# UUID를 키로 하는 연동 정보
a1b2c3d4-e5f6-7890-abcd-ef1234567890:
  playerName: "steve"           # 마인크래프트 닉네임
  platform: "CHZZK"            # 플랫폼 (CHZZK/SOOP)
  streamerId: "channelid123"    # 플랫폼 스트리머 ID
  linkedAt: 1704067200000       # 연동 시간 (타임스탬프)

b2c3d4e5-f6g7-8901-bcde-f23456789012:
  playerName: "alex"
  platform: "SOOP"
  streamerId: "bjid456"
  linkedAt: 1704153600000
```

**주의사항:**
- 이 파일은 직접 수정하지 마세요
- 연동/연동해제는 명령어를 통해서만 수행하세요
- 잘못 수정하면 연동 정보가 손실될 수 있습니다

## 🔧 고급 설정

### 성능 최적화 설정

```yaml
# config.yml에 추가 가능한 성능 설정
performance:
  cache:
    enabled: true           # 캐시 사용
    refresh-interval: 300   # 캐시 새로고침 간격 (초)
  
  event:
    max-concurrent: 10      # 최대 동시 이벤트 실행 수
    timeout: 30             # 이벤트 실행 타임아웃 (초)
    
  network:
    connect-timeout: 10     # 연결 타임아웃 (초)
    read-timeout: 30        # 읽기 타임아웃 (초)
```

### 로깅 설정

```yaml
# config.yml 로깅 섹션
logging:
  level: "INFO"        # DEBUG, INFO, WARN, ERROR
  file:
    enabled: true      # 파일 로깅 활성화
    max-size: "10MB"   # 최대 파일 크기
    max-files: 5       # 최대 파일 개수
  
  categories:
    event: true        # 이벤트 로깅
    network: false     # 네트워크 로깅
    debug: false       # 디버그 로깅
```

## 🛠️ 설정 파일 관리

### 백업 및 복원

**백업 명령어**
```bash
# 전체 설정 백업
cp -r plugins/DonationAPI plugins/DonationAPI_backup_$(date +%Y%m%d)

# 이벤트 설정만 백업
cp -r plugins/DonationAPI/events plugins/DonationAPI/events_backup_$(date +%Y%m%d)
```

**복원 방법**
1. 서버 중지
2. 백업 파일을 원래 위치로 복사
3. 서버 시작 또는 `/후원관리 리로드`

### 설정 검증

**설정 파일 문법 확인**
```bash
# YAML 문법 검증 (Linux/Mac)
python -c "import yaml; yaml.safe_load(open('plugins/DonationAPI/config.yml'))"

# 온라인 YAML 검증기 사용
# https://yamlchecker.com/
```

**설정 테스트**
```bash
# 플러그인 내에서 테스트
/후원관리 테스트 공통 1000

# 설정 리로드 후 확인
/후원관리 리로드
/후원관리 목록 공통
```

### 문제해결

**설정 파일이 로드되지 않는 경우**
1. YAML 문법 오류 확인
2. 파일 권한 확인 (644 권한 필요)
3. 파일 인코딩 확인 (UTF-8)

**이벤트가 실행되지 않는 경우**
1. 금액 설정 확인
2. 액션 타입 확인
3. 플레이어 온라인 상태 확인

**성능 문제**
1. 캐시 설정 활성화
2. 불필요한 디버그 로깅 비활성화
3. 이벤트 실행 수 제한

### 기본값으로 복원

**config.yml 초기화**
```bash
# 기존 파일 백업
mv plugins/DonationAPI/config.yml plugins/DonationAPI/config.yml.bak

# 서버 재시작하면 기본 설정 파일 자동 생성
```

**이벤트 설정 초기화**
```bash
# 이벤트 폴더 백업
mv plugins/DonationAPI/events plugins/DonationAPI/events.bak

# 새 폴더 생성 (플러그인이 자동으로 기본 파일 생성)
mkdir plugins/DonationAPI/events
```

---

**💡 설정 관리 팁:**
- 중요한 변경 전에는 항상 백업하세요
- 설정 변경 후 반드시 리로드하세요
- 복잡한 이벤트는 단계별로 테스트하세요
- YAML 들여쓰기는 스페이스 2칸을 사용하세요 