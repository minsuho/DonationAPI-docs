# 🎮 명령어 가이드

DonationAPI 플러그인의 모든 명령어를 자세히 설명합니다.

## 📋 명령어 목차

- [유저 명령어](#-유저-명령어) - 일반 플레이어가 사용하는 명령어
- [관리자 명령어](#️-관리자-명령어) - 서버 관리자가 사용하는 명령어
- [권한 시스템](#-권한-시스템) - 명령어별 필요 권한
- [자동완성](#-자동완성) - 탭 자동완성 기능

## 👤 유저 명령어

일반 플레이어가 사용할 수 있는 명령어입니다.

### `/후원연동` - 방송 연동

플레이어가 자신의 방송을 서버와 연동합니다.

#### 사용법
```bash
# 기본 형식
/후원연동 [플랫폼] [스트리머 ID]

# 치지직 연동
/후원연동 치지직 your_channel_id

# 숲 연동
/후원연동 숲 your_bjid
```

#### 플랫폼별 ID 찾는 방법

**치지직 (CHZZK)**
1. 치지직 방송 페이지 접속
2. URL에서 채널 ID 확인
   ```
   https://chzzk.naver.com/live/[채널ID]
   ```

**숲 (SOOP)**
1. 숲 방송 페이지 접속  
2. URL에서 BJ ID 확인
   ```
   https://play.afreecatv.com/[BJID]
   ```

#### 예시
```bash
# 치지직 연동 예시
/후원연동 치지직 a1b2c3d4e5f6

# 숲 연동 예시  
/후원연동 숲 mybjid123
```

#### 응답 메시지
```
✅ 성공: [DonationAPI] 치지직 방송이 성공적으로 연동되었습니다!
❌ 실패: [DonationAPI] 이미 연동된 방송이 있습니다.
❌ 실패: [DonationAPI] 올바른 플랫폼을 입력해주세요. (치지직/숲)
```

#### 필요 권한
- `donationapi.use` - 기본 사용 권한
- `donationapi.link` - 방송 연동 권한

---

### `/후원연동 정보` - 연동 정보 확인

현재 연동된 방송 정보를 확인합니다.

#### 사용법
```bash
/후원연동 정보
```

#### 응답 메시지
```
# 연동된 경우
[DonationAPI] 연동 정보:
- 플랫폼: 치지직
- 스트리머 ID: a1b2c3d4e5f6
- 연동 시간: 2024-01-15 14:30:25

# 연동되지 않은 경우
[DonationAPI] 연동된 방송이 없습니다.
```

#### 필요 권한
- `donationapi.use` - 기본 사용 권한
- `donationapi.info` - 연동 정보 확인 권한

---

## ⚙️ 관리자 명령어

서버 관리자가 사용할 수 있는 명령어입니다.

### `/후원관리` - 기본 관리 명령어

모든 관리자 명령어의 기본 형식입니다.

#### 사용법
```bash
/후원관리 [하위명령어] [옵션...]
```

#### 하위 명령어 목록
- `연동` - 플레이어 방송 연동
- `연동해제` - 플레이어 방송 연동 해제
- `이벤트설정` - 후원 이벤트 설정
- `삭제` - 이벤트 삭제
- `목록` - 이벤트 목록 확인
- `시작` - 후원 리스너 시작
- `중지` - 후원 리스너 중지
- `테스트` - 테스트 후원 실행
- `리로드` - 플러그인 리로드
- `실행설정` - 미션 실행 설정
- `대상목록` - 사용 가능한 대상 타입 확인

---

### `/후원관리 연동` - 플레이어 방송 연동

관리자가 특정 플레이어의 방송을 연동합니다.

#### 사용법
```bash
/후원관리 연동 [마크 닉네임] [플랫폼] [스트리머 ID]
```

#### 예시
```bash
# 플레이어 steve의 치지직 연동
/후원관리 연동 steve 치지직 a1b2c3d4e5f6

# 플레이어 alex의 숲 연동
/후원관리 연동 alex 숲 mybjid123
```

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 연동해제` - 방송 연동 해제

연동된 플레이어의 방송을 해제합니다.

#### 사용법
```bash
/후원관리 연동해제 [연동된 유저]
```

#### 예시
```bash
/후원관리 연동해제 steve
```

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 이벤트설정` - 후원 이벤트 설정

후원 이벤트를 설정합니다. 가장 중요하고 복잡한 명령어입니다.

#### 기본 사용법
```bash
/후원관리 이벤트설정 [대상] [금액] [타겟] [이벤트타입] [추가옵션...]
```

#### 대상 (Target)
- `공통` - 모든 스트리머에게 적용
- `치지직` - 치지직 플랫폼 전체
- `숲` - 숲 플랫폼 전체  
- `[플레이어명]` - 특정 플레이어만

#### 금액 (Amount)
- `1000` - 정확한 금액 (1000원)
- `1000~5000` - 범위 금액 (1000원~5000원)
- `무한` - 모든 금액 (기본 이벤트)

#### 타겟 (Target Type)
- `전체` - 서버 전체 온라인 플레이어
- `스트리머` - 해당 스트리머만
- `[플레이어명]` - 특정 플레이어만
- `[애드온대상]` - 애드온이 제공하는 커스텀 대상 (예: 길드멤버, VIP플레이어)

#### 이벤트 타입
- `메시지` - 메시지 출력
- `아이템` - 아이템 지급
- `랜덤아이템` - 확률 기반 랜덤 아이템 지급
- `명령어` - 명령어 실행
- `즉사` - 플레이어 즉시 사망
- `몹소환` - 확률에 따라 다양한 몹 중 하나를 소환
- `랜덤효과` - 확률에 따라 다양한 포션 효과 중 하나를 적용
- `애드온` - 애드온 실행

#### 이벤트 타입별 상세 설명

#### 📝 메시지 이벤트

**기본 메시지**
```bash
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 감사합니다!
```

**멀티라인 메시지**
```bash
/후원관리 이벤트설정 공통 5000 전체 메시지 &a첫 번째 줄\n&b두 번째 줄\n&c세 번째 줄
```

**메시지 타입별 설정**
```bash
# 채팅 메시지
/후원관리 이벤트설정 공통 1000 전체 메시지 chat:안녕하세요!

# 타이틀 메시지
/후원관리 이벤트설정 공통 2000 스트리머 메시지 title:후원 감사합니다!

# 서브타이틀 메시지  
/후원관리 이벤트설정 공통 3000 스트리머 메시지 subtitle:%donator_name%님

# 액션바 메시지
/후원관리 이벤트설정 공통 1000 전체 메시지 actionbar:후원: %amount%원

# 복합 메시지
/후원관리 이벤트설정 공통 10000 전체 메시지 chat:채팅 메시지\ntitle:타이틀 메시지\nactionbar:%amount%원
```

#### 🎁 아이템 이벤트

아이템 이벤트는 GUI를 통해 설정합니다.

```bash
# 아이템 설정 GUI 열기
/후원관리 이벤트설정 공통 1000 스트리머 아이템

# GUI에서 아이템 설정 후 저장
```

**GUI 사용법:**
1. 명령어 실행 시 GUI 창 열림
2. 원하는 아이템을 인벤토리에 넣기
3. 수량 조정
4. 저장 버튼 클릭

#### ⚡ 명령어 이벤트

**플레이어 명령어 (PLAYER:)**
```bash
# 대상 플레이어가 직접 실행
/후원관리 이벤트설정 공통 1000 스트리머 명령어 PLAYER:/tp ~ ~100 ~

# 여러 명령어 실행
/후원관리 이벤트설정 공통 5000 전체 명령어 PLAYER:/heal\nPLAYER:/feed
```

**콘솔 명령어 (CONSOLE:)**
```bash
# 콘솔에서 실행 (권한 무시)
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/give %player% diamond 1

# Op 권한 필요한 명령어
/후원관리 이벤트설정 공통 10000 전체 명령어 CONSOLE:/say %donator_name%님이 %amount%원을 후원하셨습니다!
```

**OP 권한 명령어 (OP:)**
```bash
# 대상 플레이어가 임시 OP 권한으로 실행
/후원관리 이벤트설정 공통 1000 스트리머 명령어 OP:/gamemode creative

# 전체 플레이어가 각각 OP 권한으로 실행
/후원관리 이벤트설정 공통 5000 전체 명령어 OP:/give @s diamond 64

# 여러 OP 명령어 실행
/후원관리 이벤트설정 공통 10000 스트리머 명령어 OP:/gamemode creative\nOP:/give @s netherite_sword 1
```

#### 🔧 애드온 이벤트

외부 애드온과 연동하는 이벤트입니다.

**기본 애드온 실행**
```bash
/후원관리 이벤트설정 공통 1000 스트리머 애드온 launch_to_sky
```

**변수 전달**
```bash
# key=value 형식
/후원관리 이벤트설정 공통 5000 스트리머 애드온 custom_effect power=5 duration=10

# key:value 형식  
/후원관리 이벤트설정 공통 3000 스트리머 애드온 particle_effect type:heart count:50

# 단일 key 형식
/후원관리 이벤트설정 공통 1000 전체 애드온 sound_effect loud
```

#### 🎲 랜덤 아이템 이벤트

확률에 따라 다양한 아이템 중 하나를 지급하는 이벤트입니다.

**기본 사용법**
```bash
# 랜덤 아이템 설정 GUI 열기
/후원관리 이벤트설정 공통 1000 스트리머 랜덤아이템
```

**GUI 사용법:**
1. 명령어 실행 시 랜덤 아이템 설정 GUI 창 열림
2. 원하는 아이템들을 추가
3. 각 아이템의 확률 설정 (0.1% ~ 100.0%)
4. 아이템 표시 이름 설정 (선택사항)
5. 저장

**특징:**
- 전체 확률 합은 100%를 초과할 수 없음
- 슬롯머신 스타일 애니메이션 제공
- 실시간 확률 계산 및 표시

#### ⚰️ 즉사 이벤트

대상 플레이어를 즉시 사망시키는 이벤트입니다.

**기본 사용법**
```bash
# 스트리머 즉사
/후원관리 이벤트설정 공통 1000 스트리머 즉사

# 전체 플레이어 즉사 (주의!)
/후원관리 이벤트설정 공통 10000 전체 즉사

# 특정 플레이어 즉사
/후원관리 이벤트설정 steve 5000 alex 즉사
```

**주의사항:**
- 하드코어 모드에서는 영구 밴 가능
- 전체 대상 즉사는 신중하게 사용
- PvP 비활성화 지역에서도 작동

#### 🐱 몹 소환 이벤트

확률에 따라 다양한 몹 중 하나를 소환하는 이벤트입니다.

**기본 사용법**
```bash
# 몹 소환 이벤트 설정
/후원관리 이벤트설정 공통 1000 스트리머 몹소환
```

**특징:**
- 개별 몹별 확률 설정 가능 (config.yml)
- 몹별 공격 모드 설정 가능
- 안전한 소환 위치 자동 검증
- 슬롯머신 스타일 애니메이션
- 자동 디스폰 시간 설정 가능

**config.yml에서 설정:**
- 몹별 가중치 및 소환 개수
- 몹별 공격 설정 (attack-first, force-target-spawner)
- 제외할 몹 목록
- 소환 위치 안전성 검사
- 성능 최적화 옵션

**공격 설정:**
- `attack-first: true` - 소환 즉시 공격 모드 활성화
- `force-target-spawner: true` - 후원받은 플레이어를 우선 타겟으로 설정
- 자연 스폰된 몹과는 별개로 동작

#### ✨ 랜덤 효과 이벤트

확률에 따라 다양한 포션 효과 중 하나를 적용하는 이벤트입니다.

**기본 사용법**
```bash
# 랜덤 효과 이벤트 설정
/후원관리 이벤트설정 공통 1000 스트리머 랜덤효과
```

**특징:**
- 개별 효과별 확률 설정 가능 (config.yml)
- 충돌하는 효과 자동 방지 (속도↔느려짐 등)
- 슬롯머신 스타일 애니메이션
- 해로운 효과 기본 제외

**config.yml에서 설정:**
- 효과별 가중치 및 지속시간
- 제외할 효과 목록
- 충돌 효과 조합 설정
- 성능 최적화 옵션

---

### `/후원관리 삭제` - 이벤트 삭제

설정된 이벤트를 삭제합니다.

#### 사용법
```bash
# 특정 금액의 모든 이벤트 삭제
/후원관리 삭제 [대상] [금액]

# 특정 이벤트만 삭제
/후원관리 삭제 [대상] [금액] [타겟] [이벤트타입]
```

#### 예시
```bash
# 공통 1000원 이벤트 전체 삭제
/후원관리 삭제 공통 1000

# 공통 1000원 스트리머 메시지 이벤트만 삭제
/후원관리 삭제 공통 1000 스트리머 메시지

# 범위 이벤트 삭제
/후원관리 삭제 공통 1000~5000

# 무한 이벤트 삭제
/후원관리 삭제 공통 무한
```

#### 필요 권한
- `donationapi.admin.manage` - 이벤트 관리 권한

---

### `/후원관리 목록` - 이벤트 목록 확인

설정된 이벤트 목록을 확인합니다.

#### 사용법
```bash
# 전체 목록
/후원관리 목록 [대상]

# 특정 금액만
/후원관리 목록 [대상] [금액]
```

#### 예시
```bash
# 공통 이벤트 전체 목록
/후원관리 목록 공통

# 공통 1000원 이벤트만
/후원관리 목록 공통 1000

# 플레이어별 이벤트 목록
/후원관리 목록 steve
```

#### 출력 예시
```
[DonationAPI] ═══ 공통 이벤트 목록 ═══

§e1,000원 §7이벤트
├─ 전체 대상
│  ├─ §b메시지: §f&a%donator_name%님 감사합니다!
│  └─ §6명령어: §fPLAYER:/heal
└─ 스트리머 대상
   └─ §d아이템: §f다이아몬드 x1

§e1,000~5,000원 §7이벤트  
└─ 전체 대상
   └─ §b메시지: §f범위 후원 감사합니다!

§e무한 §7이벤트
└─ 전체 대상
   └─ §b메시지: §f기본 감사 메시지
```

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 시작` - 후원 리스너 시작

후원 감지를 시작합니다.

#### 사용법
```bash
/후원관리 시작
```

#### 응답 메시지
```
✅ [DonationAPI] 후원 리스너가 시작되었습니다.
❌ [DonationAPI] 이미 실행 중입니다.
❌ [DonationAPI] API 연결에 실패했습니다.
```

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 중지` - 후원 리스너 중지

후원 감지를 중지합니다.

#### 사용법
```bash
/후원관리 중지
```

#### 응답 메시지
```
✅ [DonationAPI] 후원 리스너가 중지되었습니다.
❌ [DonationAPI] 이미 중지된 상태입니다.
```

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 테스트` - 테스트 후원 실행

실제 후원 없이 이벤트를 테스트합니다.

#### 사용법
```bash
/후원관리 테스트 [대상] [금액]
```

#### 예시
```bash
# 공통 1000원 이벤트 테스트
/후원관리 테스트 공통 1000

# 플레이어별 5000원 이벤트 테스트  
/후원관리 테스트 steve 5000

# 범위 이벤트 테스트
/후원관리 테스트 공통 3000  # 1000~5000 범위에 포함

# 무한 이벤트 테스트
/후원관리 테스트 공통 999999  # 다른 이벤트가 없으면 무한 이벤트 실행
```

#### 응답 메시지
```
✅ [DonationAPI] 테스트 후원이 실행되었습니다. (금액: 1000원)
❌ [DonationAPI] 해당 금액의 이벤트가 설정되지 않았습니다.
❌ [DonationAPI] 연동된 스트리머를 찾을 수 없습니다.
```

#### 필요 권한
- `donationapi.admin.test` - 테스트 권한

---

### `/후원관리 리로드` - 플러그인 리로드

플러그인 설정을 다시 로드합니다.

#### 사용법
```bash
/후원관리 리로드
```

#### 리로드되는 내용
- config.yml 설정
- 이벤트 설정 파일들
- 연동 정보 (linkdata.yml)
- 캐시 데이터 새로고침

#### 응답 메시지
```
✅ [DonationAPI] 플러그인이 성공적으로 리로드되었습니다.
❌ [DonationAPI] 리로드 중 오류가 발생했습니다: [오류메시지]
```

#### 필요 권한
- `donationapi.admin.reload` - 리로드 권한

---

### `/후원관리 대상목록` - 대상 타입 확인

사용 가능한 모든 대상 타입을 확인합니다.

#### 사용법
```bash
/후원관리 대상목록
```

#### 응답 메시지
```
=== 사용 가능한 대상 타입 ===

[기본 대상]
- 전체: 서버의 모든 온라인 플레이어
- 스트리머: 연동된 스트리머만
- [플레이어명]: 특정 플레이어 지정

[애드온 대상]
- 길드멤버: 스트리머와 같은 길드의 온라인 멤버들
  (제공: MyGuildPlugin)
- VIP플레이어: VIP 권한을 가진 온라인 플레이어들 (플랫폼별 권한 지원)
  (제공: DonationAPI)
  사용 가능한 변수:
    - min_amount: 최소 후원 금액 (이상일 때만 실행)
```

#### 활용 방법
- 이벤트 설정 전에 사용 가능한 대상 확인
- 애드온이 제공하는 커스텀 대상 파악
- 변수 설정이 필요한 대상 확인

#### 필요 권한
- `donationapi.admin` - 관리자 권한

---

### `/후원관리 실행설정` - 미션 실행 설정

숲 미션 시스템의 세부 설정을 조정합니다.

#### 사용법
```bash
/후원관리 실행설정 [설정항목] [값]
```

#### 설정 가능한 항목들

**정확한 매칭 설정**
```bash
# 정확한 금액만 실행 (오차 범위 무시)
/후원관리 실행설정 exact_match_only true

# 정확한 금액 우선 실행
/후원관리 실행설정 exact_match_priority true
```

**하향 오차 범위 설정**
```bash
# 하향 오차 범위 활성화
/후원관리 실행설정 down_tolerance_enabled true

# 하향 오차 범위 타입 (percent/amount)
/후원관리 실행설정 down_tolerance_type percent

# 하향 오차 범위 퍼센트 (5 = 5%)
/후원관리 실행설정 down_tolerance_percent 5

# 하향 오차 범위 금액 (100 = 100원)
/후원관리 실행설정 down_tolerance_amount 100

# 비율 실행 활성화 (금액 비례 반복 실행)
/후원관리 실행설정 down_ratio_execution true
```

**상향 오차 범위 설정**
```bash
# 상향 오차 범위 활성화
/후원관리 실행설정 up_tolerance_enabled true

# 상향 오차 범위 타입
/후원관리 실행설정 up_tolerance_type percent

# 상향 오차 범위 퍼센트
/후원관리 실행설정 up_tolerance_percent 10

# 다중 실행 활성화
/후원관리 실행설정 multiple_execution true

# 최대 실행 횟수
/후원관리 실행설정 max_events 3
```

**검색 설정**
```bash
# 검색 방향 (up/down/both)
/후원관리 실행설정 search_direction both

# 가장 가까운 것 우선
/후원관리 실행설정 closest_first true

# 중복 제거
/후원관리 실행설정 remove_duplicates true
```

**로깅 설정**
```bash
# 미션 로깅 활성화
/후원관리 실행설정 mission_logging true

# 상세 로깅
/후원관리 실행설정 detail_logging true
```

#### 현재 설정 확인
```bash
/후원관리 실행설정 목록
```

#### 필요 권한
- `donationapi.admin.manage` - 이벤트 관리 권한

---

## 🔐 권한 시스템

### 기본 권한 구조

```yaml
donationapi.*                    # 모든 권한 (관리자용)
├── donationapi.use             # 기본 사용 권한
├── donationapi.link            # 방송 연동 권한  
├── donationapi.info            # 연동 정보 확인
└── donationapi.admin.*         # 관리자 권한
    ├── donationapi.admin       # 기본 관리자 권한
    ├── donationapi.admin.manage # 이벤트 관리
    ├── donationapi.admin.reload # 리로드 권한
    └── donationapi.admin.test  # 테스트 권한
```

### 권한별 사용 가능한 명령어

| 권한 | 명령어 |
|------|--------|
| `donationapi.use` | `/후원연동`, `/후원연동 정보` |
| `donationapi.link` | `/후원연동 [플랫폼] [ID]` |
| `donationapi.info` | `/후원연동 정보` |
| `donationapi.admin` | 모든 `/후원관리` 기본 명령어 |
| `donationapi.admin.manage` | `/후원관리 이벤트설정`, `/후원관리 삭제`, `/후원관리 실행설정` |
| `donationapi.admin.reload` | `/후원관리 리로드` |
| `donationapi.admin.test` | `/후원관리 테스트` |

### LuckPerms 설정 예시

```bash
# 일반 플레이어 권한 설정
/lp group default permission set donationapi.use true
/lp group default permission set donationapi.link true  
/lp group default permission set donationapi.info true

# 스트리머 그룹 권한 (일반 플레이어 + 추가 권한)
/lp group streamer permission set donationapi.use true
/lp group streamer permission set donationapi.link true
/lp group streamer permission set donationapi.info true

# 운영진 권한
/lp group moderator permission set donationapi.admin true
/lp group moderator permission set donationapi.admin.test true

# 관리자 전체 권한
/lp group admin permission set donationapi.* true
```

## 📝 자동완성

플러그인은 모든 명령어에 대해 탭 자동완성을 지원합니다.

### 유저 명령어 자동완성

```bash
/후원연동 [TAB]
→ 치지직, 숲, 정보

/후원연동 치지직 [TAB]  
→ [스트리머 ID]
```

### 관리자 명령어 자동완성

```bash
/후원관리 [TAB]
→ 연동, 연동해제, 이벤트설정, 삭제, 목록, 시작, 중지, 테스트, 리로드, 실행설정, 대상목록

/후원관리 이벤트설정 [TAB]
→ 공통, 치지직, 숲, [연동된 플레이어 목록]

/후원관리 이벤트설정 공통 [TAB]
→ [숫자], 무한, [기존 설정된 금액들]

/후원관리 이벤트설정 공통 1000 [TAB]  
→ 전체, 스트리머, [온라인 플레이어 목록], [애드온 대상 목록]

/후원관리 이벤트설정 공통 1000 전체 [TAB]
→ 메시지, 아이템, 명령어, 애드온

/후원관리 이벤트설정 공통 1000 전체 메시지 [TAB]
→ chat:, title:, subtitle:, actionbar:, \n

/후원관리 이벤트설정 공통 1000 전체 명령어 [TAB]
→ console:, player:, \n, [기존 명령어들]

/후원관리 이벤트설정 공통 1000 전체 애드온 [TAB]
→ [애드온 이름], [변수]
```

### 동적 자동완성

- **연동된 플레이어**: 현재 연동된 플레이어 목록을 자동으로 표시
- **온라인 플레이어**: 현재 온라인인 플레이어 목록 표시  
- **기존 설정값**: 이미 설정된 금액이나 명령어를 표시
- **변수 예시**: 애드온 변수 설정 시 예시 제공

---

**💡 Tip**: 명령어 사용 중 언제든지 TAB 키를 눌러 사용 가능한 옵션을 확인할 수 있습니다! 