# Snort 룰 작성 - 실기 기출 및 예상 문제 (최빈출!)

## 🔴 Snort 룰 기본 구조 (필수 암기!)

```
action protocol src_ip src_port direction dst_ip dst_port (rule options)
```

### 예시 분석
```
alert tcp any any -> 192.168.1.0/24 80 (msg:"HTTP GET Request"; content:"GET"; sid:1000001; rev:1;)
│     │   │   │    │  │               │
│     │   │   │    │  │               └─ 룰 옵션
│     │   │   │    │  └─ 목적지 포트
│     │   │   │    └─ 방향 지시자
│     │   │   └─ 출발지 포트
│     │   └─ 출발지 IP
│     └─ 프로토콜
└─ 액션
```

---

## 🔴 SID (Signature ID) 범위 - 필수 암기!

```
┌───────────┬──────────────┬─────────────┐
│   범위     │      용도     │    사용자    │
├───────────┼──────────────┼─────────────┤
│  0-99     │ 시스템 예약   │   Snort     │
├───────────┼──────────────┼─────────────┤
│100-999,999│ Snort 공식   │   Snort     │
│           │     룰       │             │
├───────────┼──────────────┼─────────────┤
│1,000,000+ │ 사용자 정의   │ 관리자/사용자 │
│           │     룰       │             │
└───────────┴──────────────┴─────────────┘
```

**🔴 실기 시험에서 룰 작성 시:**
- **항상 1000000 이상** 사용!
- 예: sid:1000001, sid:1000010, sid:2000001

---

## 📝 기출 유형 1: SSH Brute Force 탐지

### 문제 1-1 (2023년 실기 - 최빈출!)
```
다음 조건에 맞는 Snort 룰을 작성하시오.

조건:
- 임의의 출발지에서 내부 네트워크(192.168.1.0/24)의
  22번 포트로 접속 시도
- 30초 내에 동일한 출발지에서 5회 이상 시도하면 경고
- 메시지: "SSH Brute Force Attack"
- SID: 1000010
```

**✅ 답안:**
```
alert tcp any any -> 192.168.1.0/24 22 (msg:"SSH Brute Force Attack"; threshold:type threshold, track by_src, count 5, seconds 30; sid:1000010; rev:1;)
```

**📌 상세 분석:**
```
alert tcp                      # TCP 프로토콜에 대해 경고
any any                        # 모든 출발지 IP:포트
->                            # 단방향
192.168.1.0/24 22             # 목적지 네트워크:SSH 포트

msg:"SSH Brute Force Attack"  # 경고 메시지
threshold:                    # 임계값 설정
  type threshold              # threshold 타입
  track by_src                # 출발지 IP 추적
  count 5                     # 5회
  seconds 30                  # 30초 이내
sid:1000010                   # 사용자 정의 SID
rev:1                         # 룰 버전
```

---

### 문제 1-2 (변형 문제)
```
FTP Brute Force 공격 탐지 룰을 작성하시오.
- 목적지: 10.0.0.0/8 네트워크
- 포트: 21 (FTP)
- 조건: 1분 내 10회 접속 시도
- 메시지: "FTP Brute Force Detected"
- SID: 1000020
```

**✅ 답안:**
```
alert tcp any any -> 10.0.0.0/8 21 (msg:"FTP Brute Force Detected"; threshold:type threshold, track by_src, count 10, seconds 60; sid:1000020; rev:1;)
```

---

## 📝 기출 유형 2: Ping of Death 탐지

### 문제 2-1 (2022년 실기)
```
Ping of Death 공격을 탐지하는 Snort 룰을 작성하시오.

조건:
- ICMP 패킷
- 패킷 크기 1000바이트 초과
- 2초 내 동일 출발지에서 10개 이상 패킷
- 목적지: 192.168.10.0/24
- 메시지: "Ping of Death Attack"
- SID: 1000030
```

**✅ 답안:**
```
alert icmp any any -> 192.168.10.0/24 any (msg:"Ping of Death Attack"; dsize:>1000; threshold:type threshold, track by_src, count 10, seconds 2; sid:1000030; rev:1;)
```

**📌 주요 옵션:**
```
dsize:>1000   # 데이터 크기가 1000바이트 초과
              # > : 초과
              # < : 미만
              # : : 정확히

threshold:    # 임계값 설정
  type threshold
  track by_src
  count 10
  seconds 2
```

---

## 📝 기출 유형 3: Port Scan 탐지

### 문제 3-1 (FIN Scan)
```
FIN Scan 공격을 탐지하는 룰을 작성하시오.

조건:
- TCP FIN 플래그만 설정된 패킷
- 5초 내 동일 출발지에서 10회 이상
- 목적지: $HOME_NET (모든 내부 네트워크)
- 메시지: "FIN Scan Detected"
- SID: 1000040
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET any (msg:"FIN Scan Detected"; flags:F; threshold:type threshold, track by_src, count 10, seconds 5; sid:1000040; rev:1;)
```

**📌 TCP 플래그:**
```
flags:S     # SYN
flags:SA    # SYN+ACK
flags:F     # FIN
flags:R     # RST
flags:P     # PSH
flags:A     # ACK
flags:U     # URG

예시:
flags:F     → FIN만 설정 (FIN Scan)
flags:FPU   → FIN+PSH+URG (XMAS Scan)
flags:!A    → ACK가 설정되지 않음
```

---

### 문제 3-2 (XMAS Scan)
```
XMAS Scan 탐지 룰을 작성하시오.
- FIN, PSH, URG 플래그가 모두 설정된 패킷
- 5초 내 10회
- SID: 1000050
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET any (msg:"XMAS Scan Detected"; flags:FPU; threshold:type threshold, track by_src, count 10, seconds 5; sid:1000050; rev:1;)
```

---

### 문제 3-3 (NULL Scan)
```
NULL Scan 탐지 룰을 작성하시오.
- 모든 플래그가 0인 패킷
- SID: 1000060
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET any (msg:"NULL Scan Detected"; flags:0; threshold:type threshold, track by_src, count 10, seconds 5; sid:1000060; rev:1;)
```

---

## 📝 기출 유형 4: SQL Injection 탐지

### 문제 4-1
```
SQL Injection 공격을 탐지하는 룰을 작성하시오.

조건:
- HTTP 트래픽 (80번 포트)
- 페이로드에 "' OR '1'='1" 문자열 포함
- 대소문자 구분 안 함
- 메시지: "SQL Injection Attempt"
- SID: 1000070
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET 80 (msg:"SQL Injection Attempt"; content:"' OR '1'='1"; nocase; sid:1000070; rev:1;)
```

**📌 content 옵션:**
```
content:"GET"           # "GET" 문자열 포함
nocase                  # 대소문자 구분 안 함
depth:10                # 처음 10바이트 내에서 검색
offset:5                # 5바이트 이후부터 검색
distance:0              # 이전 content로부터 상대 위치
within:20               # 이전 content로부터 20바이트 내

예시:
content:"Host|3a 20|";  # "Host: " (16진수 표현)
                        # 3a = ':'
                        # 20 = ' '
```

---

### 문제 4-2 (UNION SQL Injection)
```
UNION 기반 SQL Injection 탐지 룰
- "UNION SELECT" 문자열 포함
- SID: 1000071
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET 80 (msg:"UNION SQL Injection"; content:"UNION"; nocase; content:"SELECT"; nocase; distance:0; within:20; sid:1000071; rev:1;)
```

---

## 📝 기출 유형 5: HTTP GET Flooding

### 문제 5-1
```
HTTP GET Flooding 공격 탐지 룰 작성

조건:
- HTTP 요청 (GET 메서드)
- 패킷 시작 부분 3바이트 내에 "GET" 포함
- 10초 내 동일 출발지에서 100회 이상
- 메시지: "HTTP GET Flooding"
- SID: 1000080
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET 80 (msg:"HTTP GET Flooding"; content:"GET"; depth:3; threshold:type threshold, track by_src, count 100, seconds 10; sid:1000080; rev:1;)
```

---

## 📝 기출 유형 6: Heartbleed 공격

### 문제 6-1 (고급 문제)
```
Heartbleed 취약점 공격 탐지 룰 작성

조건:
- HTTPS (443 포트)
- 패킷 시작부터 2바이트 내에 "|18 03|" 포함
  (18 = Heartbeat, 03 = TLS 1.2)
- 3바이트 떨어진 곳 1바이트 내에 "|01|" 포함
- 메시지: "Heartbleed Attack Detected"
- SID: 1000090
```

**✅ 답안:**
```
alert tcp any any -> $HOME_NET 443 (msg:"Heartbleed Attack Detected"; content:"|18 03|"; depth:2; content:"|01|"; distance:3; within:1; sid:1000090; rev:1;)
```

**📌 16진수 표현:**
```
content:"|18 03|"     # 16진수 바이트 직접 지정
                       # 18 (10진수 24) = Heartbeat
                       # 03 (10진수 3)  = TLS 1.2

content:"|0d 0a|"     # CRLF (Carriage Return + Line Feed)
                       # 0d = CR (\r)
                       # 0a = LF (\n)
```

---

## 📝 기출 유형 7: 룰 수정 및 최적화

### 문제 7-1
```
다음 Snort 룰의 문제점을 찾고 수정하시오.

alert tcp any any -> 192.168.1.0/24 22 (msg:"SSH Attack"; threshold:type threshold, track by_src, count 5, seconds 30; sid:100; rev:1;)
```

**✅ 답안:**
```
문제점:
sid:100 → 공식 Snort 룰 범위 (100-999,999)
사용자 정의 룰은 1,000,000 이상 사용해야 함

수정:
alert tcp any any -> 192.168.1.0/24 22 (msg:"SSH Attack"; threshold:type threshold, track by_src, count 5, seconds 30; sid:1000100; rev:1;)
```

---

### 문제 7-2
```
다음 룰이 작동하지 않는 이유를 설명하고 수정하시오.

alert tcp any any -> $HOME_NET 80 (msg:"SQL Injection"; content:"' OR '1'='1"; sid:1000001;)
```

**✅ 답안:**
```
문제점:
rev (룰 버전) 누락

수정:
alert tcp any any -> $HOME_NET 80 (msg:"SQL Injection"; content:"' OR '1'='1"; sid:1000001; rev:1;)

설명:
Snort 룰에는 msg, sid, rev가 필수 요소입니다.
- msg: 경고 메시지
- sid: 룰 고유 ID
- rev: 룰 버전 (보통 1부터 시작)
```

---

## 🎯 Snort 룰 작성 체크리스트

### 필수 요소 확인
```
✅ action (alert, log, drop 등)
✅ protocol (tcp, udp, icmp, ip)
✅ src_ip, src_port
✅ direction (-> 또는 <>)
✅ dst_ip, dst_port
✅ msg (경고 메시지)
✅ sid (1000000 이상!) ★★★
✅ rev (룰 버전, 보통 1)
```

### 자주 사용하는 옵션
```
threshold       → 임계값 설정 (Brute Force, DoS)
content         → 문자열 검색
nocase          → 대소문자 무시
flags           → TCP 플래그 (Port Scan)
dsize           → 패킷 크기 (Ping of Death)
depth           → 검색 깊이
distance        → 이전 content로부터 거리
within          → 범위 내
flow            → 연결 상태 (established 등)
```

---

## 📊 그림으로 이해하기

### threshold 옵션 시각화
```
[30초 동안 출발지 추적, 5회 이상이면 경고]

시간(초): 0....5....10....15....20....25....30
IP 1.1.1.1: X    X    X     X     X     X
            1    2    3     4     5     ↑
                                        경고!

threshold:type threshold, track by_src, count 5, seconds 30
           │                  │           │         │
           └─ 타입             │           │         └─ 시간 범위
                              │           └─ 횟수
                              └─ 출발지 IP 추적
```

### content + depth/offset
```
패킷 데이터:
[G E T   / i n d e x . h t m l   H T T P ...]
 0 1 2 3 4 5 6 7 8 9 ...

content:"GET"; depth:3
→ 처음 3바이트(0-2) 내에서 "GET" 검색

content:"index.html"; offset:5
→ 5바이트 이후부터 검색
```

### 다중 content (distance/within)
```
content:"UNION"; nocase; content:"SELECT"; distance:0; within:20;

[... U N I O N   S E L E C T ...]
      ↑           ↑
      1번째       2번째 content
                  distance:0 (바로 다음)
      └──────────┘
         within:20 (20바이트 내)
```

---

## 🔥 실기 시험 최빈출 룰 유형 (꼭 연습!)

```
1위: SSH/FTP Brute Force   ← threshold 사용
2위: Port Scan 탐지        ← flags 사용
3위: Ping of Death         ← dsize 사용
4위: SQL Injection         ← content 사용
5위: HTTP GET Flooding     ← depth + threshold
```

---

## 💡 실기 답안 작성 팁

### Tip 1: SID는 항상 1000000 이상!
```
✅ sid:1000001;
✅ sid:1000010;
✅ sid:2000001;
❌ sid:100;      (공식 룰 범위)
❌ sid:999999;   (공식 룰 범위)
```

### Tip 2: 세미콜론 위치 주의
```
✅ sid:1000001; rev:1;)
❌ sid:1000001, rev:1;)   (쉼표 사용 불가)
❌ sid:1000001; rev:1)    (마지막 세미콜론 누락)
```

### Tip 3: 괄호 짝 맞추기
```
✅ alert tcp any any -> $HOME_NET 80 (msg:"Test"; sid:1000001; rev:1;)
                                     ↑                               ↑
                                     여는 괄호                  닫는 괄호
```

### Tip 4: threshold 형식 정확히
```
✅ threshold:type threshold, track by_src, count 5, seconds 30;
❌ threshold:type=threshold, track=by_src, count=5, seconds=30;
   (= 기호 사용 불가)
```

### Tip 5: 문자열은 쌍따옴표
```
✅ msg:"SSH Attack";
✅ content:"GET";
❌ msg:'SSH Attack';    (작은따옴표 불가)
```

---

**✅ 이 유형들만 연습하면 Snort 룰 문제는 거의 다 맞출 수 있습니다!**

**실전 연습: 각 유형을 직접 손으로 3번씩 써보세요!**
