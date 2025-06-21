# 🔧 애드온 개발 가이드

DonationAPI의 애드온 시스템을 활용하여 커스텀 이벤트를 개발하는 방법을 설명합니다.

## 📋 목차

- [애드온 개요](#-애드온-개요)
- [Action 인터페이스](#-action-인터페이스)
- [애드온 개발 단계](#-애드온-개발-단계)
- [실제 구현 예제](#-실제-구현-예제)
- [고급 기능](#-고급-기능)
- [배포 및 등록](#-배포-및-등록)

## 🎯 애드온 개요

### 애드온이란?
DonationAPI의 기본 이벤트 외에 추가로 개발할 수 있는 커스텀 액션입니다.

### 애드온 시스템 특징
- **플러그인 호환성**: 다른 플러그인과 완벽 연동
- **변수 전달**: 후원 데이터를 애드온으로 전달
- **간단한 등록**: actions HashMap에 추가만으로 등록 완료
- **독립적 실행**: 각 애드온은 독립적으로 동작

### 애드온 활용 예시
- 파티클 효과 생성
- 사운드 재생
- 몹 소환
- 날씨 변경
- 커스텀 GUI 표시
- 외부 API 호출

## 🏗️ Action 인터페이스

### 기본 인터페이스 구조
```java
package com.gitter.donationAPI.actions;

import org.bukkit.entity.Player;
import com.gitter.donationAPI.models.DonationData;
import java.util.Map;

public interface Action {
    /**
     * 애드온 액션을 실행합니다.
     * 
     * @param player 대상 플레이어 (null일 수 있음)
     * @param data 후원 데이터
     * @param variables 전달받은 변수들
     * @return 실행 성공 여부
     */
    boolean execute(Player player, DonationData data, Map<String, String> variables);
    
    /**
     * 애드온의 이름을 반환합니다.
     * 
     * @return 애드온 이름
     */
    String getName();
    
    /**
     * 애드온의 설명을 반환합니다.
     * 
     * @return 애드온 설명 (선택사항)
     */
    default String getDescription() {
        return "커스텀 애드온";
    }
}
```

### DonationData 구조
```java
public class DonationData {
    private String platform;        // "CHZZK" 또는 "SOOP"
    private String streamerId;      // 스트리머 ID
    private String donatorName;     // 후원자 닉네임
    private String donatorId;       // 후원자 ID
    private int amount;            // 후원 금액
    private String message;        // 후원 메시지
    private int count;             // 후원 개수
    private long timestamp;        // 후원 시간
    
    // Getter 메서드들...
}
```

## 🛠️ 애드온 개발 단계

### 1단계: 프로젝트 설정

**build.gradle 의존성 추가**
```gradle
dependencies {
    compileOnly files('libs/DonationAPI.jar')
    compileOnly 'org.spigotmc:spigot-api:1.20.1-R0.1-SNAPSHOT'
}
```

### 2단계: Action 클래스 구현

**기본 템플릿**
```java
package com.yourname.yourplugin.actions;

import com.gitter.donationAPI.actions.Action;
import com.gitter.donationAPI.models.DonationData;
import org.bukkit.entity.Player;

import java.util.Map;

public class YourCustomAction implements Action {
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        try {
            // 여기에 커스텀 로직 구현
            
            return true; // 성공 시 true 반환
        } catch (Exception e) {
            e.printStackTrace();
            return false; // 실패 시 false 반환
        }
    }
    
    @Override
    public String getName() {
        return "your_custom_action"; // 애드온 이름 (명령어에서 사용)
    }
    
    @Override
    public String getDescription() {
        return "당신의 커스텀 애드온 설명";
    }
}
```

### 3단계: DonationAPI에 등록

**메인 클래스에서 등록**
```java
package com.yourname.yourplugin;

import com.gitter.donationAPI.actions.API;
import org.bukkit.plugin.java.JavaPlugin;

public class YourPlugin extends JavaPlugin {
    
    @Override
    public void onEnable() {
        // DonationAPI가 로드된 후 등록
        getServer().getScheduler().runTaskLater(this, () -> {
            registerActions();
        }, 20L); // 1초 후 등록
    }
    
    private void registerActions() {
        // DonationAPI에 애드온 등록
        API.registerAction("your_custom_action", new YourCustomAction());
        
        getLogger().info("애드온이 DronationAPI에 등록되었습니다!");
    }
}
```

### 4단계: 사용법
```bash
# 기본 실행
/후원관리 이벤트설정 공통 1000 스트리머 애드온 your_custom_action

# 변수와 함께 실행
/후원관리 이벤트설정 공통 5000 스트리머 애드온 your_custom_action power=10 duration=30
```

## 🚀 실제 구현 예제

### 예제 1: 파티클 효과 애드온

```java
package com.example.actions;

import com.gitter.donationAPI.actions.Action;
import com.gitter.donationAPI.models.DonationData;
import org.bukkit.Color;
import org.bukkit.Location;
import org.bukkit.Particle;
import org.bukkit.entity.Player;

import java.util.Map;

public class ParticleEffectAction implements Action {
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        if (player == null || !player.isOnline()) {
            return false;
        }
        
        try {
            // 변수에서 파티클 타입과 개수 가져오기
            String particleType = variables.getOrDefault("type", "HEART");
            int count = Integer.parseInt(variables.getOrDefault("count", "10"));
            double radius = Double.parseDouble(variables.getOrDefault("radius", "2.0"));
            
            Location loc = player.getLocation().add(0, 1, 0);
            Particle particle = Particle.valueOf(particleType);
            
            // 원형으로 파티클 생성
            for (int i = 0; i < count; i++) {
                double angle = 2 * Math.PI * i / count;
                double x = Math.cos(angle) * radius;
                double z = Math.sin(angle) * radius;
                
                Location particleLoc = loc.clone().add(x, 0, z);
                player.getWorld().spawnParticle(particle, particleLoc, 1);
            }
            
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }
    
    @Override
    public String getName() {
        return "particle_effect";
    }
    
    @Override
    public String getDescription() {
        return "플레이어 주변에 파티클 효과를 생성합니다";
    }
}
```

**사용법:**
```bash
# 기본 하트 파티클
/후원관리 이벤트설정 공통 1000 스트리머 애드온 particle_effect

# 커스텀 파티클
/후원관리 이벤트설정 공통 5000 스트리머 애드온 particle_effect type=FLAME count=20 radius=3.0
```

### 예제 2: 랜덤 몹 소환 애드온

```java
package com.example.actions;

import com.gitter.donationAPI.actions.Action;
import com.gitter.donationAPI.models.DonationData;
import org.bukkit.Location;
import org.bukkit.entity.EntityType;
import org.bukkit.entity.LivingEntity;
import org.bukkit.entity.Player;

import java.util.Map;
import java.util.Random;

public class RandomMobSpawnAction implements Action {
    
    private final EntityType[] FRIENDLY_MOBS = {
        EntityType.COW, EntityType.PIG, EntityType.SHEEP, 
        EntityType.CHICKEN, EntityType.HORSE
    };
    
    private final EntityType[] HOSTILE_MOBS = {
        EntityType.ZOMBIE, EntityType.SKELETON, EntityType.CREEPER,
        EntityType.SPIDER, EntityType.ENDERMAN
    };
    
    private final Random random = new Random();
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        if (player == null || !player.isOnline()) {
            return false;
        }
        
        try {
            String mobType = variables.getOrDefault("type", "friendly");
            int count = Integer.parseInt(variables.getOrDefault("count", "1"));
            int radius = Integer.parseInt(variables.getOrDefault("radius", "5"));
            
            EntityType[] mobArray = mobType.equals("hostile") ? HOSTILE_MOBS : FRIENDLY_MOBS;
            
            for (int i = 0; i < count; i++) {
                // 랜덤 위치 계산
                Location spawnLoc = player.getLocation().add(
                    random.nextInt(radius * 2) - radius,
                    0,
                    random.nextInt(radius * 2) - radius
                );
                
                // 지면 찾기
                spawnLoc = spawnLoc.getWorld().getHighestBlockAt(spawnLoc).getLocation().add(0, 1, 0);
                
                // 랜덤 몹 선택 및 소환
                EntityType mobToSpawn = mobArray[random.nextInt(mobArray.length)];
                LivingEntity entity = (LivingEntity) spawnLoc.getWorld().spawnEntity(spawnLoc, mobToSpawn);
                
                // 커스텀 이름 설정 (선택사항)
                if (variables.containsKey("name")) {
                    entity.setCustomName(variables.get("name").replace("%donator%", data.getDonatorName()));
                    entity.setCustomNameVisible(true);
                }
            }
            
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }
    
    @Override
    public String getName() {
        return "random_mob_spawn";
    }
    
    @Override
    public String getDescription() {
        return "플레이어 주변에 랜덤 몹을 소환합니다";
    }
}
```

**사용법:**
```bash
# 친화적인 몹 1마리
/후원관리 이벤트설정 공통 2000 스트리머 애드온 random_mob_spawn

# 적대적인 몹 3마리
/후원관리 이벤트설정 공통 10000 스트리머 애드온 random_mob_spawn type=hostile count=3

# 커스텀 이름으로 소환
/후원관리 이벤트설정 공통 5000 스트리머 애드온 random_mob_spawn name=%donator%의_선물
```

### 예제 3: 경제 연동 애드온

```java
package com.example.actions;

import com.gitter.donationAPI.actions.Action;
import com.gitter.donationAPI.models.DonationData;
import net.milkbowl.vault.economy.Economy;
import org.bukkit.Bukkit;
import org.bukkit.entity.Player;
import org.bukkit.plugin.RegisteredServiceProvider;

import java.util.Map;

public class EconomyRewardAction implements Action {
    
    private Economy economy;
    
    public EconomyRewardAction() {
        setupEconomy();
    }
    
    private boolean setupEconomy() {
        if (Bukkit.getPluginManager().getPlugin("Vault") == null) {
            return false;
        }
        
        RegisteredServiceProvider<Economy> rsp = Bukkit.getServicesManager()
            .getRegistration(Economy.class);
        
        if (rsp == null) {
            return false;
        }
        
        economy = rsp.getProvider();
        return economy != null;
    }
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        if (player == null || !player.isOnline() || economy == null) {
            return false;
        }
        
        try {
            // 후원 금액의 비율로 게임머니 지급
            double ratio = Double.parseDouble(variables.getOrDefault("ratio", "0.1"));
            double rewardAmount = data.getAmount() * ratio;
            
            // 최소/최대 금액 제한
            double minAmount = Double.parseDouble(variables.getOrDefault("min", "0"));
            double maxAmount = Double.parseDouble(variables.getOrDefault("max", "999999"));
            
            rewardAmount = Math.max(minAmount, Math.min(maxAmount, rewardAmount));
            
            // 게임머니 지급
            economy.depositPlayer(player, rewardAmount);
            
            // 메시지 전송
            String message = variables.getOrDefault("message", 
                "&a후원 보상으로 &e%amount%원&a을 받았습니다!");
            
            message = message.replace("%amount%", String.format("%.0f", rewardAmount));
            player.sendMessage(message.replace("&", "§"));
            
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }
    
    @Override
    public String getName() {
        return "economy_reward";
    }
    
    @Override
    public String getDescription() {
        return "후원 금액에 비례하여 게임머니를 지급합니다";
    }
}
```

**사용법:**
```bash
# 후원 금액의 10%를 게임머니로 지급
/후원관리 이벤트설정 공통 1000 스트리머 애드온 economy_reward

# 커스텀 비율 및 제한
/후원관리 이벤트설정 공통 5000 스트리머 애드온 economy_reward ratio=0.2 min=100 max=10000
```

## 🎯 고급 기능

### 비동기 처리

```java
public class AsyncAction implements Action {
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        // 비동기 작업이 필요한 경우
        Bukkit.getScheduler().runTaskAsynchronously(plugin, () -> {
            // 무거운 작업 (API 호출, 파일 처리 등)
            doHeavyWork();
            
            // 메인 스레드에서 실행해야 하는 작업
            Bukkit.getScheduler().runTask(plugin, () -> {
                // Bukkit API 사용
                player.sendMessage("작업 완료!");
            });
        });
        
        return true; // 비동기 시작 성공
    }
}
```

### 다중 플레이어 처리

```java
public class MultiPlayerAction implements Action {
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        String target = variables.getOrDefault("target", "streamer");
        
        switch (target.toLowerCase()) {
            case "all":
                // 모든 온라인 플레이어
                for (Player p : Bukkit.getOnlinePlayers()) {
                    executeForPlayer(p, data, variables);
                }
                break;
                
            case "random":
                // 랜덤 플레이어
                List<Player> players = new ArrayList<>(Bukkit.getOnlinePlayers());
                if (!players.isEmpty()) {
                    Player randomPlayer = players.get(new Random().nextInt(players.size()));
                    executeForPlayer(randomPlayer, data, variables);
                }
                break;
                
            default:
                // 기본: 스트리머만
                if (player != null) {
                    executeForPlayer(player, data, variables);
                }
                break;
        }
        
        return true;
    }
    
    private void executeForPlayer(Player p, DonationData data, Map<String, String> variables) {
        // 개별 플레이어 처리 로직
    }
}
```

### 설정 파일 연동

```java
public class ConfigurableAction implements Action {
    
    private FileConfiguration config;
    
    public ConfigurableAction(Plugin plugin) {
        // 설정 파일 로드
        File configFile = new File(plugin.getDataFolder(), "addon-config.yml");
        config = YamlConfiguration.loadConfiguration(configFile);
    }
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        // 설정값 사용
        boolean enabled = config.getBoolean("features.particle-effect", true);
        String soundName = config.getString("sounds.reward", "ENTITY_PLAYER_LEVELUP");
        
        // 설정에 따른 실행
        if (enabled) {
            player.playSound(player.getLocation(), Sound.valueOf(soundName), 1.0f, 1.0f);
        }
        
        return true;
    }
}
```

## 📦 배포 및 등록

### 1. JAR 파일 빌드
```bash
./gradlew build
```

### 2. DonationAPI에 등록

**플러그인 로드 후 등록**
```java
@Override
public void onEnable() {
    // DonationAPI 로드 확인
    if (Bukkit.getPluginManager().getPlugin("DonationAPI") == null) {
        getLogger().warning("DonationAPI가 필요합니다!");
        Bukkit.getPluginManager().disablePlugin(this);
        return;
    }
    
    // 1초 후 등록 (DonationAPI 완전 로드 대기)
    Bukkit.getScheduler().runTaskLater(this, this::registerActions, 20L);
}

private void registerActions() {
    // 여러 애드온 한번에 등록
    API.registerAction("particle_effect", new ParticleEffectAction());
    API.registerAction("random_mob_spawn", new RandomMobSpawnAction());
    API.registerAction("economy_reward", new EconomyRewardAction());
    
    getLogger().info("모든 애드온이 등록되었습니다!");
}
```

### 3. plugin.yml 설정
```yaml
name: YourAddonPlugin
version: 1.0.0
main: com.yourname.YourPlugin
api-version: 1.20
depend: [DonationAPI]
softdepend: [Vault, PlaceholderAPI]

description: DonationAPI 애드온 플러그인
author: YourName
```

### 4. 사용자 가이드 작성

**README.md 예시**
```markdown
# DonationAPI 애드온 플러그인

## 설치 방법
1. DonationAPI가 먼저 설치되어 있어야 합니다
2. JAR 파일을 plugins 폴더에 복사
3. 서버 재시작

## 애드온 목록
- `particle_effect` - 파티클 효과
- `random_mob_spawn` - 랜덤 몹 소환  
- `economy_reward` - 경제 시스템 연동

## 사용법
```bash
/후원관리 이벤트설정 공통 1000 스트리머 애드온 particle_effect type=HEART count=10
```

## 💡 개발 팁

### 디버깅
```java
public class DebugAction implements Action {
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        // 디버그 정보 출력
        Bukkit.getLogger().info("=== 애드온 디버그 ===");
        Bukkit.getLogger().info("플레이어: " + (player != null ? player.getName() : "null"));
        Bukkit.getLogger().info("후원 금액: " + data.getAmount());
        Bukkit.getLogger().info("후원자: " + data.getDonatorName());
        Bukkit.getLogger().info("변수들: " + variables.toString());
        
        return true;
    }
}
```

### 에러 처리
```java
@Override
public boolean execute(Player player, DonationData data, Map<String, String> variables) {
    try {
        // 필수 체크
        if (player == null) {
            Bukkit.getLogger().warning("플레이어가 null입니다");
            return false;
        }
        
        if (!player.isOnline()) {
            Bukkit.getLogger().warning("플레이어가 오프라인입니다: " + player.getName());
            return false;
        }
        
        // 메인 로직
        doMainLogic();
        
        return true;
    } catch (Exception e) {
        Bukkit.getLogger().severe("애드온 실행 중 오류: " + e.getMessage());
        e.printStackTrace();
        return false;
    }
}
```

### 성능 최적화
```java
public class OptimizedAction implements Action {
    
    // 한 번만 생성하여 재사용
    private static final Random RANDOM = new Random();
    private static final Map<String, Sound> SOUND_CACHE = new HashMap<>();
    
    @Override
    public boolean execute(Player player, DonationData data, Map<String, String> variables) {
        // 무거운 작업은 캐시 활용
        Sound sound = SOUND_CACHE.computeIfAbsent(
            variables.getOrDefault("sound", "ENTITY_PLAYER_LEVELUP"),
            soundName -> {
                try {
                    return Sound.valueOf(soundName);
                } catch (IllegalArgumentException e) {
                    return Sound.ENTITY_PLAYER_LEVELUP; // 기본값
                }
            }
        );
        
        player.playSound(player.getLocation(), sound, 1.0f, 1.0f);
        return true;
    }
}
```

## 🚀 다음 단계

1. **GitHub에 소스코드 공개** - 다른 개발자들과 공유
2. **SpigotMC에 업로드** - 더 많은 사용자에게 배포
3. **DonationAPI 디스코드에 공유** - 공식 애드온으로 등록 가능

---

**💡 Tip**: 애드온 개발 시 막히는 부분이 있다면 DonationAPI 디스코드에서 도움을 요청하세요!

**📞 문의**: [DonationAPI 디스코드](https://discord.gg/Sr42sSM7kr) 