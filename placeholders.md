# 🏷️ 플레이스홀더 가이드

DonationAPI에서 사용할 수 있는 모든 플레이스홀더를 상세히 설명합니다.

## 📋 목차

- [플레이스홀더란?](#-플레이스홀더란)
- [기본 플레이스홀더](#-기본-플레이스홀더)
- [플랫폼 플레이스홀더](#-플랫폼-플레이스홀더)
- [후원자 플레이스홀더](#-후원자-플레이스홀더)
- [스트리머 플레이스홀더](#-스트리머-플레이스홀더)
- [금액/수량 플레이스홀더](#-금액수량-플레이스홀더)
- [메시지 플레이스홀더](#-메시지-플레이스홀더)
- [사용 예제](#-사용-예제)
- [고급 활용법](#-고급-활용법)

## 🎯 플레이스홀더란?

플레이스홀더는 실제 후원 데이터로 자동 치환되는 변수입니다.

### 기본 문법
```
%변수명% → 실제 데이터로 치환
```

### 사용 가능한 위치
- 메시지 이벤트의 텍스트
- 명령어 이벤트의 명령어
- 아이템 이벤트의 이름/설명 (설정된 경우)

## 🌟 기본 플레이스홀더

### `%platform%` - 플랫폼 이름
후원이 들어온 플랫폼의 이름입니다.

**가능한 값:**
- `CHZZK` - 치지직
- `SOOP` - 숲

**예시:**
```bash
# 메시지에서 사용
/후원관리 이벤트설정 공통 1000 전체 메시지 &a[%platform%] 후원 감사합니다!

# 결과:
# 치지직 후원 시: "[CHZZK] 후원 감사합니다!"
# 숲 후원 시: "[SOOP] 후원 감사합니다!"
```

### `%platform_ko%` - 플랫폼 한국어 이름
후원이 들어온 플랫폼의 한국어 이름입니다.

**가능한 값:**
- `치지직` - 치지직 플랫폼
- `숲` - 숲 플랫폼

**예시:**
```bash
# 메시지에서 사용
/후원관리 이벤트설정 공통 1000 전체 메시지 &a[%platform_ko%] 후원 감사합니다!

# 결과:
# 치지직 후원 시: "[치지직] 후원 감사합니다!"
# 숲 후원 시: "[숲] 후원 감사합니다!"
```

### `%streamer_id%` - 스트리머 ID
연동된 스트리머의 플랫폼 ID입니다.

**값 설명:**
- 치지직: 채널 ID
- 숲: BJ ID

**예시:**
```bash
# 명령어에서 사용
/후원관리 이벤트설정 공통 1000 전체 명령어 CONSOLE:/say 스트리머 %streamer_id%에게 후원이 들어왔습니다!

# 결과:
# "스트리머 mybjid123에게 후원이 들어왔습니다!"
```

## 👤 후원자 플레이스홀더

### `%donator_name%` - 후원자 표시 이름
후원자의 플랫폼 표시 이름입니다.

**특징:**
- 치지직: 닉네임
- 숲: 닉네임
- 익명 후원 시: "익명" 또는 "Anonymous"

**예시:**
```bash
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 감사합니다!

# 결과: "홍길동님 감사합니다!"
# 익명: "익명님 감사합니다!"
```

### `%donator_id%` - 후원자 실제 ID
후원자의 플랫폼 실제 ID입니다.

**특징:**
- 치지직: 사용자 ID
- 숲: 사용자 ID
- 익명이나 ID가 없는 경우: `null` 또는 빈 문자열

**예시:**
```bash
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/say 후원자 ID: %donator_id%

# 결과: "후원자 ID: user123456"
# 익명: "후원자 ID: "
```

## 🎮 스트리머 플레이스홀더

### `%player%` - 스트리머 마인크래프트 닉네임
연동된 스트리머의 마인크래프트 닉네임입니다.

**용도:**
- 명령어에서 대상 지정
- 메시지에서 스트리머 언급

**예시:**
```bash
# 명령어에서 대상 지정
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/give %player% diamond 1

# 메시지에서 언급
/후원관리 이벤트설정 공통 1000 전체 메시지 &a스트리머 %player%님에게 후원이 들어왔습니다!

# 결과: "스트리머 steve님에게 후원이 들어왔습니다!"
```

### `%uuid%` - 스트리머 UUID
스트리머의 마인크래프트 UUID입니다.

**용도:**
- 고급 명령어에서 정확한 플레이어 지정
- 데이터베이스 연동
- 플러그인 간 연동

**예시:**
```bash
# UUID를 이용한 정확한 지정
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/lp user %uuid% permission set special.donor true

# 결과: "/lp user a1b2c3d4-e5f6-7890-abcd-ef1234567890 permission set special.donor true"
```

## 💰 금액/수량 플레이스홀더

### `%amount%` - 후원 금액
후원된 금액입니다.

**특징:**
- 단위: 원 (KRW)
- 소수점 가능
- 미션 후원 시: 원래 후원 금액

**예시:**
```bash
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%amount%원 후원 감사합니다!

# 결과: "5000원 후원 감사합니다!"
```

### `%cnt%` - 후원 개수
후원 개수입니다.

**특징:**
- 치지직: 도네이션 개수
- 숲: 별풍선/후원 개수
- 일반적으로 1개

**예시:**
```bash
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%cnt%개의 후원을 받으셨습니다!

# 결과: "1개의 후원을 받으셨습니다!"
# 도전미션 성공 시: "5개의 후원을 받으셨습니다!"
```

### `%count%` - `%cnt%`의 별칭
`%cnt%`와 동일한 기능입니다.

### `%value%` - 설정된 단위에 맞는 값
config.yml의 `event-type` 설정에 따라 적절한 값을 출력합니다.

**설정별 출력:**
- `event-type: "amount"` → 후원 금액 (%amount%와 동일)
- `event-type: "count"` → 후원 개수 (%cnt%와 동일)

**예시:**
```bash
# config.yml에서 event-type: "amount"인 경우
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%value% 후원 감사합니다!
# 결과: "5000 후원 감사합니다!"

# config.yml에서 event-type: "count"인 경우  
/후원관리 이벤트설정 공통 5 전체 메시지 &a%value% 후원 감사합니다!
# 결과: "5 후원 감사합니다!"
```

### `%unit%` - 설정된 단위
config.yml의 `event-type` 설정에 따라 적절한 단위를 출력합니다.

**설정별 출력:**
- `event-type: "amount"` → `원`
- `event-type: "count"` → `개`

**예시:**
```bash
# config.yml에서 event-type: "amount"인 경우
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%value%%unit% 후원 감사합니다!
# 결과: "5000원 후원 감사합니다!"

# config.yml에서 event-type: "count"인 경우
/후원관리 이벤트설정 공통 5 전체 메시지 &a%value%%unit% 후원 감사합니다!
# 결과: "5개 후원 감사합니다!"
```

### `%value%`와 `%unit%` 조합 활용
```bash
# 범용적인 메시지 템플릿
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님이 %value%%unit%의 후원을 하셨습니다!

# 금액 모드: "홍길동님이 5000원의 후원을 하셨습니다!"
# 개수 모드: "홍길동님이 5개의 후원을 하셨습니다!"
```

## 💬 메시지 플레이스홀더

### `%message%` - 후원 메시지
후원자가 남긴 메시지입니다.

**특징:**
- 메시지가 없는 경우: 빈 문자열
- 특수문자 포함 가능
- 줄바꿈 문자 포함 가능

**예시:**
```bash
/후원관리 이벤트설정 공통 1000 전체 메시지 &a후원 메시지: &f%message%

# 메시지 있음: "후원 메시지: 안녕하세요! 방송 잘 보고 있어요!"
# 메시지 없음: "후원 메시지: "
```

### 메시지 활용 팁
```bash
# 메시지가 있을 때만 표시
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 후원 감사합니다!%message%

# 메시지 길이 제한 (플러그인 외부에서 처리 필요)
# 조건부 메시지 표시는 별도 플러그인 필요
```

## 📊 사용 예제

### 기본 메시지 템플릿
```bash
# 간단한 감사 메시지
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 %amount%원 후원 감사합니다!

# 플랫폼 포함 메시지 (영문)
/후원관리 이벤트설정 공통 1000 전체 메시지 &9[%platform%] &a%donator_name%님이 %amount%원을 후원하셨습니다!

# 플랫폼 포함 메시지 (한국어)
/후원관리 이벤트설정 공통 1000 전체 메시지 &9[%platform_ko%] &a%donator_name%님이 %amount%원을 후원하셨습니다!

# 범용 메시지 (설정에 따라 자동 변경)
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님이 %value%%unit%의 후원을 하셨습니다!

# 메시지 포함 템플릿
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 후원 감사합니다!\n&7메시지: &f%message%
```

### 타이틀 메시지 활용
```bash
# 타이틀에 후원자 이름
/후원관리 이벤트설정 공통 5000 스트리머 메시지 title:&6%donator_name%님\nsubtitle:&e%amount%원 후원 감사합니다!

# 액션바에 간단 정보
/후원관리 이벤트설정 공통 1000 전체 메시지 actionbar:&c후원: %donator_name% - %amount%원
```

### 명령어에서 활용
```bash
# 스트리머에게 아이템 지급
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/give %player% diamond %cnt%

# 전체 공지
/후원관리 이벤트설정 공통 5000 전체 명령어 CONSOLE:/say [%platform%] %donator_name%님이 %amount%원을 후원하셨습니다!

# 경험치 지급 (금액 비례)
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/xp add %player% %amount%

# 돈 지급 (Vault 플러그인 필요)
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/eco give %player% %amount%
```

### 복합 활용
```bash
# 여러 정보를 포함한 상세 메시지
/후원관리 이벤트설정 공통 10000 전체 메시지 chat:&4&l★ 대형 후원 ★\nchat:&6후원자: &f%donator_name%\nchat:&6금액: &f%amount%원\nchat:&6플랫폼: &f%platform%\nchat:&6스트리머: &f%player%\nchat:&6메시지: &f%message%\ntitle:&6대형 후원!\nsubtitle:&e%donator_name%님 감사합니다!
```

## 🚀 고급 활용법

### 조건부 메시지 (외부 플러그인 연동)
```bash
# ConditionalEvents 플러그인 예시
condition: '%amount% >= 10000'
actions:
  - 'message: &4&l특별 후원! %donator_name%님이 %amount%원을 후원하셨습니다!'

# PlaceholderAPI 연동 예시  
/후원관리 이벤트설정 공통 1000 전체 메시지 &a%donator_name%님 후원 감사합니다! (서버 시간: %server_time%)
```

### 데이터베이스 연동
```bash
# MySQL 플러그인 연동 (가상 예시)
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/mysql execute "INSERT INTO donations (player, donator, amount, message) VALUES ('%player%', '%donator_name%', %amount%, '%message%')"

# 통계 업데이트
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/mysql execute "UPDATE player_stats SET total_donations = total_donations + %amount% WHERE player = '%player%'"
```

### 외부 API 연동
```bash
# Discord 웹훅 (DiscordSRV 예시)
/후원관리 이벤트설정 공통 5000 전체 명령어 CONSOLE:/discord send webhook "후원 알림: %donator_name%님이 %player%님에게 %amount%원을 후원하셨습니다!"

# 웹사이트 알림
/후원관리 이벤트설정 공통 10000 전체 명령어 CONSOLE:/web notify "donation" "donator=%donator_name%&amount=%amount%&streamer=%player%"
```

### 복잡한 명령어 조합
```bash
# 여러 단계 이벤트
/후원관리 이벤트설정 공통 20000 스트리머 명령어 CONSOLE:/give %player% diamond_block 1\nCONSOLE:/title %player% title {"text":"특별 후원!","color":"gold"}\nCONSOLE:/title %player% subtitle {"text":"%donator_name%님 감사합니다!","color":"yellow"}\nCONSOLE:/effect give %player% glowing 30 1\nCONSOLE:/playsound entity.player.levelup master %player% ~ ~ ~ 1 1

# 조건부 실행 (CommandHelper 등)
/후원관리 이벤트설정 공통 1000 스트리머 명령어 CONSOLE:/ch run if(%amount% > 5000, give(%player%, diamond, %amount%/1000), give(%player%, iron_ingot, 1))
```

## ⚠️ 주의사항

### 특수문자 처리
```bash
# 따옴표가 포함된 메시지
후원 메시지: 안녕하세요 "스트리머"님!
→ 명령어에서 오류 발생 가능

# 해결방법: 명령어에서는 메시지 사용 시 주의
# 또는 메시지 필터링 플러그인 사용
```

### null 값 처리
```bash
# 익명 후원 시 처리
%donator_name% → "익명" 또는 "Anonymous"
%donator_id% → null 또는 빈 문자열

# 메시지가 없을 때
%message% → 빈 문자열
```

### 플레이스홀더 대소문자
```bash
# 정확한 대소문자 사용 필요
%donator_name% ✅ 올바름
%DONATOR_NAME% ❌ 작동하지 않음
%Donator_Name% ❌ 작동하지 않음
```

### 플랫폼별 차이점
```bash
# 치지직 (CHZZK) - 공식 API
%platform% → "CHZZK"
%donator_name% → 치지직 닉네임
%streamer_id% → 채널 ID (영문+숫자)
후원 유형: 치즈 (미션 미지원)

# 숲 (SOOP) - 중계 API
%platform% → "SOOP"  
%donator_name% → 숲 닉네임
%streamer_id% → BJ ID (영문+숫자)
후원 유형: 별풍선, 애드벌룬, 도전미션, 대결미션
```

## 📝 플레이스홀더 치트시트

| 플레이스홀더 | 설명 | 예시 값 |
|-------------|------|---------|
| `%platform%` | 플랫폼 이름 | `CHZZK`, `SOOP` |
| `%platform_ko%` | 플랫폼 한국어 이름 | `치지직`, `숲` |
| `%streamer_id%` | 스트리머 플랫폼 ID | `channelid123`, `bjid456` |
| `%player%` | 마인크래프트 닉네임 | `steve`, `alex` |
| `%uuid%` | 마인크래프트 UUID | `a1b2c3d4-e5f6-...` |
| `%donator_name%` | 후원자 이름 | `홍길동`, `익명` |
| `%donator_id%` | 후원자 플랫폼 ID | `user123456` |
| `%amount%` | 후원 금액 | `1000`, `5000.5` |
| `%cnt%`, `%count%` | 후원 개수 | `1`, `5` |
| `%value%` | 설정된 단위에 맞는 값 | `5000` (금액모드), `5` (개수모드) |
| `%unit%` | 설정된 단위 | `원` (금액모드), `개` (개수모드) |
| `%message%` | 후원 메시지 | `안녕하세요!` |

---

**💡 활용 팁:**
- 메시지에서는 모든 플레이스홀더 사용 가능
- 명령어에서는 특수문자 주의
- 플랫폼별 특성을 고려한 메시지 작성
- 외부 플러그인과 연동으로 더 풍부한 기능 구현 가능