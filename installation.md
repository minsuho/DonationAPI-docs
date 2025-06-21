# 📦 설치 가이드

DonationAPI 플러그인의 설치 방법과 초기 설정을 안내합니다.

## 🔧 시스템 요구사항

### 필수 요구사항
- **Java**: 17 이상
- **Minecraft 서버**: Spigot 1.20.1 (1.19 이하는 별도 문의)
- **메모리**: 최소 1GB RAM
- **네트워크**: 인터넷 연결 (Socket.IO 통신), Velocity 지원

### 지원하지 않는 환경
- ❌ BungeeCord/Waterfall 네트워크
- ❌ Bukkit 서버
- ❌ 1.18 이하 버전 (1.19는 별도 문의)

## 📥 플러그인 다운로드

1. **공식 디스코드**에서 플러그인 파일을 받으세요
   - Discord: https://discord.gg/Sr42sSM7kr
   - 파일명: `DonationAPI-x.x.x.jar`

## 🚀 설치 과정

### 1단계: 플러그인 파일 복사
```bash
# 서버 폴더 구조
minecraft-server/
├── plugins/
│   └── DonationAPI-x.x.x.jar  ← 여기에 복사
├── server.jar
└── config.yml
```

### 2단계: 서버 재시작
```bash
# 서버 중지
/stop

# 서버 시작
java -jar server.jar
```

### 3단계: 설정 파일 확인
서버 시작 후 다음 파일들이 자동 생성됩니다:
```bash
plugins/DonationAPI/
├── config.yml          # 메인 설정 파일
├── linkdata.yml         # 연동 정보 저장
├── events/
│   ├── common.yml       # 공통 이벤트
│   ├── platforms/       # 플랫폼별 이벤트
│   │   ├── chzzk.yml   # 치지직 이벤트
│   │   └── soop.yml    # 숲 이벤트
│   └── users/          # 개별 스트리머 이벤트
│       └── [UUID].yml  # 개별 스트리머 이벤트
└── logs/
    └── donation.log     # 후원 로그
```

## ⚙️ 초기 설정

### 1. API 키 설정
`plugins/DonationAPI/config.yml` 파일을 편집하세요:

```yaml
# API 설정
api:
  key: "YOUR_API_KEY_HERE"    # ← 여기에 발급받은 API 키 입력
  url: "http://121.145.190.195:3000"
  
# 소켓 설정  
socket:
  url: "http://121.145.190.195:3001"
  reconnect-interval: 5
```

### 2. 기본 설정 확인
```yaml
# 이벤트 설정
events:
  async: true
  execution-delay: 1
  event-type: "amount"    # "amount" 또는 "count"

# 디버그 설정
debug:
  enabled: false
  detail: false
  
# 미션 설정
mission:
  enabled: true
  exact_match:
    only: false
    priority: true
```

### 3. 권한 설정
서버 권한 플러그인(LuckPerms, GroupManager 등)에서 권한을 설정하세요:

```yaml
# 일반 플레이어 권한
permissions:
  - donationapi.use          # 기본 명령어 사용
  - donationapi.link         # 방송 연동
  - donationapi.info         # 연동 정보 확인

# 관리자 권한  
admin-permissions:
  - donationapi.admin        # 관리자 명령어
  - donationapi.admin.manage # 이벤트 관리
  - donationapi.admin.reload # 리로드
  - donationapi.admin.test   # 테스트 기능
```

## 🔐 API 키 발급

### 스트리머 (무료)
1. 디스코드 서버 접속
2. 스트리머 인증 진행
3. API 키 발급 요청
4. 심사 후 API 키 제공

### 일반 서버 (유료)
1. 디스코드 서버 접속
2. 결제 진행 (월 5,000원)
3. API 키 즉시 발급

## 🧪 설치 확인

### 1. 플러그인 로드 확인
```bash
# 콘솔에서 확인
/plugins

# 결과 예시
[INFO] Plugins (1): DonationAPI
```

### 2. 명령어 작동 확인
```bash
# 게임 내에서 확인
/후원연동

# 결과 예시
[DonationAPI] 사용법: /후원연동 [치지직/숲] [스트리머 ID]
```

### 3. API 연결 확인
```bash
# 관리자 명령어로 확인
/후원관리 시작

# 결과 예시
[DonationAPI] 후원 리스너가 시작되었습니다.
```

## 🚦 첫 번째 연동

### 1. 방송 연동
```bash
# 치지직 연동
/후원연동 치지직 your_channel_id

# 숲 연동  
/후원연동 숲 your_bjid
```

### 2. 테스트 이벤트 설정
```bash
# 간단한 메시지 이벤트 설정
/후원관리 이벤트설정 공통 1000 전체 메시지 &a후원 감사합니다!
```

### 3. 테스트 실행
```bash
# 테스트 후원 실행
/후원관리 테스트 공통 1000

# 결과 확인
[DonationAPI] 테스트 후원이 실행되었습니다.
```

## ❗ 설치 문제해결

### 플러그인이 로드되지 않는 경우
1. **Java 버전 확인**
   ```bash
   java -version
   # Java 17 이상인지 확인
   ```

2. **파일 권한 확인**
   ```bash
   # Linux/Mac
   chmod 644 DonationAPI-x.x.x.jar
   ```

3. **서버 버전 확인**
   ```bash
   # Spigot 1.20.1인지 확인
   /version
   ```

### API 연결 실패
1. **방화벽 설정 확인**
   - 포트 3000, 3001 아웃바운드 허용

2. **네트워크 연결 확인**
   ```bash
   # 연결 테스트
   ping 121.145.190.195
   ```

3. **API 키 확인**
   - 올바른 API 키 입력 여부
   - 공백이나 특수문자 포함 여부

### 명령어가 작동하지 않는 경우
1. **권한 설정 확인**
   ```bash
   # 권한 확인 명령어 (LuckPerms 예시)
   /lp user [플레이어] permission check donationapi.use
   ```

2. **플러그인 의존성 확인**
   ```bash
   # 필요한 플러그인이 있는지 확인
   /plugins
   ```

## 🌐 Velocity 네트워크 설정

### Velocity 프록시 설정
DonationAPI는 Velocity 네트워크를 지원합니다.

#### 설치 방법
1. **Velocity 프록시에 플러그인 설치**
   ```bash
   velocity/plugins/DonationAPI-Velocity.jar
   ```

2. **config.yml에서 Velocity 모드 활성화**
   ```yaml
   # config.yml
   velocity:
     enabled: true    # Velocity 모드 활성화
   ```

3. **백엔드 서버에도 DonationAPI 설치**
   ```bash
   survival/plugins/DonationAPI.jar
   creative/plugins/DonationAPI.jar
   ```

4. **완료!**
   - 각 서버에서 후원 처리
   - 전체 이벤트만 Velocity를 통해 네트워크 전달

#### 작동 방식
- **각 서버**: 후원 처리 및 스트리머/특정 플레이어 이벤트 실행
- **Velocity 플러그인**: **전체 대상 이벤트만** 네트워크 전체로 전달
- **우편함 시스템**: 전체 아이템 지급 시 우편함으로 전달 가능

### 이벤트 처리 방식

#### 각 서버에서 처리되는 이벤트
- **스트리머 대상**: 해당 스트리머가 있는 서버에서만 실행
- **특정 플레이어 대상**: 해당 플레이어가 있는 서버에서만 실행
- **명령어**: 각 서버의 콘솔/플레이어 명령어

#### Velocity를 통해 전달되는 이벤트 (전체 대상만)
- **전체 대상 메시지**: 모든 서버의 모든 플레이어에게 전송
- **전체 대상 아이템**: 우편함을 통해 모든 서버의 모든 플레이어에게 전송
- **전체 대상 명령어**: 네트워크 전체에서 실행

### 주의사항
- Velocity 전용 플러그인 파일 필요
- BungeeCord/Waterfall은 지원하지 않습니다
- 각 백엔드 서버에도 개별 설치 필요
- 단일 API 키로 전체 네트워크 관리

## 📞 추가 지원

설치 중 문제가 발생하면 다음 정보와 함께 문의하세요:

### 필요한 정보
- 서버 버전 (`/version`)
- Java 버전 (`java -version`)
- 네트워크 구성 (Velocity/단일서버)
- 에러 로그 (콘솔 출력)
- 설정 파일 내용

### 특별 문의 사항
- **1.19 이하 버전**: 별도 호환성 문의
- **Velocity 네트워크**: 설정 지원 문의
- **서버팩**: 라이센스 문의

### 문의 방법
- **Discord**: https://discord.gg/Sr42sSM7kr
---

**✅ 설치 완료!** 이제 [명령어 가이드](commands.md)를 참조하여 플러그인을 사용해보세요. 