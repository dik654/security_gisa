# KISA Windows 취약점 점검 항목 - 실기 필수 암기

## 🔴 반드시 암기해야 하는 Top 10 취약점 항목

### W-01: 최신 보안 패치 및 서비스팩 설치 ★★★
```
점검 내용:
Windows OS 및 응용 프로그램의 최신 보안 패치가 적용되어 있는지 점검

취약점 발생 조건:
- 중요 보안 업데이트 미적용
- 자동 업데이트 비활성화

점검 방법:
1. GUI: 제어판 → Windows Update → 업데이트 기록 보기
2. CMD: wmic qfe list
3. PowerShell: Get-HotFix

보안 대책:
1. Windows Update 설정
   - 제어판 → Windows Update
   - "자동으로 업데이트 설치(권장)" 선택

2. WSUS(Windows Server Update Services) 사용
   - 기업 환경에서 중앙 집중식 패치 관리

3. 정기적인 패치 점검 (월 1회 이상)
```

**📝 기출 문제 1-1 (2023년 실기)**
```
시스템에 설치된 모든 보안 패치(핫픽스) 목록을
확인하는 명령어를 쓰시오.
```

**✅ 답안:**
```
wmic qfe list

또는

Get-HotFix (PowerShell)
```

---

### W-03: 불필요한 서비스 제거 ★★
```
점검 내용:
불필요하거나 사용하지 않는 서비스가 실행 중인지 점검

주요 점검 대상 서비스:
- Telnet (포트 23)
- FTP (포트 21)
- TFTP (포트 69)
- SNMP (포트 161)
- RDP (불필요 시)

점검 방법:
1. GUI: services.msc
2. CMD: net start
3. PowerShell: Get-Service | Where-Object {$_.Status -eq "Running"}

보안 대책:
불필요한 서비스 중지 및 비활성화

1. GUI 방법:
   services.msc → 서비스 우클릭 → 속성
   → 시작 유형: "사용 안 함"
   → 서비스 상태: "중지"

2. CMD 방법:
   sc config "서비스명" start= disabled
   sc stop "서비스명"

3. PowerShell 방법:
   Stop-Service -Name "서비스명"
   Set-Service -Name "서비스명" -StartupType Disabled
```

**📝 기출 문제 3-1**
```
Telnet 서비스를 중지하고 시작 유형을 "사용 안 함"으로
설정하는 sc 명령어를 쓰시오.
```

**✅ 답안:**
```
sc stop Telnet
sc config Telnet start= disabled

(주의: start= 다음에 공백 필요!)
```

---

### W-04: 관리자 그룹에 최소한의 사용자 포함 ★★★
```
점검 내용:
Administrators 그룹에 불필요한 사용자가 포함되어 있는지 점검

취약점 발생 조건:
- 일반 사용자를 Administrators 그룹에 추가
- 관리자 권한 남용 위험

점검 방법:
1. GUI: lusrmgr.msc → 그룹 → Administrators 더블클릭
2. CMD: net localgroup Administrators
3. PowerShell: Get-LocalGroupMember -Group "Administrators"

보안 대책:
불필요한 사용자 제거

net localgroup Administrators 사용자명 /delete

권장 정책:
- 업무상 필요한 최소한의 관리자만 포함
- 일반 사용자는 Users 그룹 사용
- 필요 시 "권한 상승" 사용 (UAC)
```

**📝 기출 문제 4-1 (2022년 실기)**
```
Administrators 그룹의 구성원을 확인하는 명령어를 쓰시오.
```

**✅ 답안:**
```
net localgroup Administrators
```

---

### W-06: 불필요한 계정 제거 ★★
```
점검 내용:
장기간 미사용 계정 및 불필요한 계정이 존재하는지 점검

점검 대상:
- Guest 계정 (기본 제공, 비활성화 필요)
- 퇴사자 계정
- 테스트 계정
- 90일 이상 미사용 계정

점검 방법:
1. 모든 계정 확인
   net user

2. 특정 계정 상세 정보
   net user 사용자명

3. Guest 계정 상태 확인
   net user Guest

보안 대책:
1. Guest 계정 비활성화
   net user Guest /active:no

2. 불필요한 계정 삭제
   net user 사용자명 /delete

3. 미사용 계정 잠금
   net user 사용자명 /active:no
```

**📝 기출 문제 6-1 (2023년 실기)**
```
Guest 계정을 비활성화하는 명령어를 쓰시오.
```

**✅ 답안:**
```
net user Guest /active:no
```

---

### W-07: 계정 잠금 임계값 설정 ★★★
```
점검 내용:
로그인 연속 실패 시 계정 잠금 정책이 설정되어 있는지 점검

취약점 발생 조건:
- 계정 잠금 임계값 "0" (잠금 안 함)
- Brute Force 공격에 취약

점검 방법:
secpol.msc → 계정 정책 → 계정 잠금 정책

보안 대책:
설정 항목 (권장값):
1. 계정 잠금 임계값: 5회
2. 계정 잠금 기간: 60분
3. 계정 잠금 카운터 다시 설정: 60분

설정 방법:
secpol.msc → 계정 정책 → 계정 잠금 정책
→ 각 항목 더블클릭하여 설정

또는 명령어:
net accounts /lockoutthreshold:5
net accounts /lockoutduration:60
net accounts /lockoutwindow:60
```

**📝 기출 문제 7-1 (2022년 실기)**
```
다음 조건에 맞게 계정 잠금 정책을 설정하는 명령어를 쓰시오.

- 5회 로그인 실패 시 계정 잠금
- 잠금 기간: 60분
- 잠금 카운터 재설정: 60분
```

**✅ 답안:**
```
net accounts /lockoutthreshold:5
net accounts /lockoutduration:60
net accounts /lockoutwindow:60
```

**📌 관련 이벤트 ID:**
```
4625: 로그온 실패 (각 실패 시도)
4740: 사용자 계정 잠금 (임계값 도달)
4767: 사용자 계정 잠금 해제
```

---

### W-08: 패스워드 복잡성 설정 ★★★
```
점검 내용:
패스워드 복잡성 요구 정책이 활성화되어 있는지 점검

취약점 발생 조건:
- 복잡성 요구 사항 비활성화
- 단순 패스워드 사용 가능

점검 방법:
secpol.msc → 계정 정책 → 암호 정책
→ "암호는 복잡성을 만족해야 함"

보안 대책:
1. 암호 복잡성 정책 설정
   - 암호는 복잡성을 만족해야 함: 사용
   - 최소 암호 길이: 8자 이상
   - 최대 암호 사용 기간: 90일
   - 최소 암호 사용 기간: 1일
   - 암호 기록 보관: 5개

2. 복잡성 요구 사항:
   - 영문 대문자 포함
   - 영문 소문자 포함
   - 숫자 포함
   - 특수 문자 포함 (!@#$%^&* 등)
   - 사용자 계정 이름 포함 금지

명령어:
net accounts /minpwlen:8
net accounts /maxpwage:90
net accounts /minpwage:1
net accounts /uniquepw:5
```

**📝 기출 문제 8-1 (2023년 실기)**
```
패스워드 최소 길이를 8자로 설정하고,
최대 사용 기간을 90일로 설정하는 명령어를 쓰시오.
```

**✅ 답안:**
```
net accounts /minpwlen:8
net accounts /maxpwage:90
```

---

### W-15: 백신 프로그램 업데이트 ★★
```
점검 내용:
백신 프로그램이 설치되어 있고 최신 버전으로 업데이트되어 있는지 점검

취약점 발생 조건:
- 백신 미설치
- 백신 정의 파일 구버전 (최신 악성코드 탐지 불가)

점검 항목:
1. 백신 프로그램 설치 여부
2. 백신 엔진 버전
3. 바이러스 정의 파일 업데이트 날짜
4. 실시간 감시 활성화 여부
5. 정기 검사 스케줄 설정

보안 대책:
1. 백신 프로그램 설치
   - 공공기관: GS 인증 백신 (V3, 알약 등)
   - 기업: Symantec, McAfee, Trend Micro 등

2. 자동 업데이트 설정
   - 바이러스 정의 파일: 일 1회 이상
   - 엔진 업데이트: 자동 설정

3. 정기 검사 스케줄
   - 전체 검사: 주 1회
   - 빠른 검사: 일 1회
```

---

### W-18: Administrator 계정 이름 바꾸기 ★★
```
점검 내용:
기본 Administrator 계정명을 변경했는지 점검

취약점 발생 조건:
- 기본 계정명(Administrator) 사용
- 공격자가 관리자 계정명을 쉽게 유추

보안 대책:
1. GUI 방법:
   lusrmgr.msc → 사용자 → Administrator 우클릭
   → 이름 바꾸기 → 새 이름 입력 (예: Admin_sys01)

2. 보안 정책 방법:
   secpol.msc → 로컬 정책 → 보안 옵션
   → "계정: Administrator 계정 이름 바꾸기"
   → 새 이름 입력

권장 사항:
- 추측하기 어려운 이름 사용
- "admin", "root" 등 유추 가능한 이름 금지
- 정기적으로 변경 (분기 1회)
```

**📝 기출 문제 18-1**
```
기본 Administrator 계정명을 변경하는 보안 정책 설정
위치를 쓰시오.
```

**✅ 답안:**
```
secpol.msc → 로컬 정책 → 보안 옵션
→ 계정: Administrator 계정 이름 바꾸기
```

---

### W-19: Guest 계정 이름 바꾸기 ★
```
점검 내용:
기본 Guest 계정명을 변경했는지 점검

보안 대책:
1. Guest 계정 비활성화 (W-06)
   net user Guest /active:no

2. 계정명 변경
   secpol.msc → 로컬 정책 → 보안 옵션
   → "계정: Guest 계정 이름 바꾸기"

권장:
Guest 계정은 비활성화하는 것이 최선
```

---

### W-22: 백업 파일 안전한 보관 ★★
```
점검 내용:
1. 백업 파일이 정기적으로 생성되는지 점검
2. 백업 파일이 안전한 장소에 보관되는지 점검
3. 백업 복구 테스트를 수행하는지 점검

취약점 발생 조건:
- 백업 미실시
- 백업 파일이 동일 시스템에 저장 (재해 시 함께 손실)
- 백업 파일 암호화 미적용

보안 대책:
1. 백업 정책 수립
   - 전체 백업: 주 1회
   - 증분 백업: 일 1회
   - 백업 보관: 최소 1개월

2. 백업 저장 위치
   - 물리적으로 분리된 장소
   - 오프사이트 백업 (원격지)
   - 클라우드 백업

3. 백업 암호화
   - AES-256 이상
   - 압축 파일에 패스워드 설정

4. 복구 테스트
   - 분기별 1회 이상 복구 테스트 수행
```

**📝 기출 문제 22-1**
```
백업 데이터 보관 시 반드시 준수해야 할 3가지 보안 원칙을
쓰시오.
```

**✅ 답안:**
```
1. 물리적 분리: 원본과 백업을 다른 장소에 보관
2. 암호화: 백업 데이터를 암호화하여 저장
3. 정기 테스트: 백업 복구 테스트를 정기적으로 수행
```

---

## 📝 종합 문제: KISA Windows 취약점 점검 시나리오

### 종합 문제 1 (2023년 실기 유사)
```
다음 요구사항에 따라 Windows 시스템의 보안을 강화하시오.

1) Guest 계정을 비활성화
2) 로그인 5회 실패 시 계정 잠금, 잠금 기간 30분 설정
3) 패스워드 최소 길이 10자로 설정
4) Administrators 그룹 구성원 확인

각 요구사항에 대해 명령어 또는 설정 방법을 쓰시오.
```

**✅ 답안:**
```
1) Guest 계정 비활성화
   net user Guest /active:no

2) 계정 잠금 정책
   net accounts /lockoutthreshold:5
   net accounts /lockoutduration:30

3) 패스워드 최소 길이
   net accounts /minpwlen:10

4) Administrators 그룹 구성원 확인
   net localgroup Administrators
```

---

### 종합 문제 2 (점검 및 조치)
```
다음은 Windows Server 2019 점검 결과이다.
취약점과 조치 방법을 쓰시오.

C:\> net user Guest
계정 활성화              예
...

C:\> net accounts
최소 암호 길이              0
최대 암호 사용 기간         무제한
계정 잠금 임계값            사용 안 함
```

**✅ 답안:**
```
취약점 1: Guest 계정 활성화
- 위험: 익명 사용자 접근 가능
- 조치: net user Guest /active:no

취약점 2: 최소 암호 길이 0
- 위험: 빈 패스워드 허용
- 조치: net accounts /minpwlen:8

취약점 3: 최대 암호 사용 기간 무제한
- 위험: 패스워드 변경 없이 무기한 사용 가능
- 조치: net accounts /maxpwage:90

취약점 4: 계정 잠금 임계값 사용 안 함
- 위험: Brute Force 공격에 취약
- 조치: net accounts /lockoutthreshold:5
        net accounts /lockoutduration:60
```

---

### 종합 문제 3 (이벤트 로그 분석)
```
다음 Windows 보안 로그를 분석하고 문제점과 대응 방안을 쓰시오.

이벤트 ID: 4625 (로그온 실패)
계정: Administrator
시간: 2024-11-05 14:30:15
원본: 203.0.113.50

이벤트 ID: 4625
계정: Administrator
시간: 2024-11-05 14:30:20
원본: 203.0.113.50

이벤트 ID: 4625
계정: Administrator
시간: 2024-11-05 14:30:25
원본: 203.0.113.50

(동일 패턴 50회 반복)
```

**✅ 답안:**
```
1. 문제점 분석:
   - 공격 유형: Brute Force Attack (무차별 대입 공격)
   - 대상 계정: Administrator (기본 관리자 계정)
   - 공격 출발지: 203.0.113.50
   - 특징: 5초 간격으로 연속 50회 로그인 실패

2. 취약점:
   - Administrator 기본 계정명 사용 (W-18 취약)
   - 계정 잠금 정책 미설정 (W-07 취약)

3. 대응 방안:
   즉시 조치:
   - 방화벽에서 공격 IP 차단
     netsh advfirewall firewall add rule name="Block Attack"
     dir=in action=block remoteip=203.0.113.50

   - 해당 IP의 모든 활동 로그 확인
     이벤트 뷰어에서 203.0.113.50 필터링

   근본 대책:
   - Administrator 계정명 변경
     secpol.msc → 보안 옵션 → 계정명 변경

   - 계정 잠금 정책 설정
     net accounts /lockoutthreshold:5
     net accounts /lockoutduration:60

   - IPS/IDS 룰 추가 (Brute Force 탐지)
```

---

## 🎯 KISA Windows 취약점 점검 핵심 암기 카드

### 카드 1: 계정 관리 명령어 (최빈출!)
```
Guest 비활성화:
net user Guest /active:no

계정 생성:
net user 사용자명 패스워드 /add

계정 삭제:
net user 사용자명 /delete

그룹 구성원 확인:
net localgroup Administrators

그룹에 사용자 추가:
net localgroup Administrators 사용자명 /add
```

### 카드 2: 패스워드 정책
```
net accounts /minpwlen:8          # 최소 길이 8자
net accounts /maxpwage:90         # 최대 90일
net accounts /minpwage:1          # 최소 1일
net accounts /uniquepw:5          # 이전 5개 기록
net accounts /lockoutthreshold:5  # 5회 실패
net accounts /lockoutduration:60  # 60분 잠금
```

### 카드 3: 서비스 관리
```
서비스 중지:
sc stop 서비스명

서비스 시작 유형 변경:
sc config 서비스명 start= disabled
(주의: start= 다음 공백 필수!)

서비스 목록:
sc query state= all
```

### 카드 4: 보안 점검 도구
```
secpol.msc    → 로컬 보안 정책
lusrmgr.msc   → 로컬 사용자 및 그룹
services.msc  → 서비스 관리
eventvwr.msc  → 이벤트 뷰어
gpedit.msc    → 그룹 정책 편집기
```

### 카드 5: 주요 이벤트 ID (W-62 관련)
```
4624 → 로그온 성공
4625 → 로그온 실패 (Brute Force 탐지)
4672 → 관리자 권한 로그온
4720 → 사용자 계정 생성
4740 → 사용자 계정 잠금
```

---

## 📊 그림으로 이해하기

### Windows 보안 점검 프로세스
```
[1단계: 시스템 정보 확인]
   systeminfo
   └─ OS 버전, 설치 날짜, 핫픽스 목록

[2단계: 계정 점검]
   net user
   └─ 모든 계정 목록
   └─ Guest 계정 활성화 여부
   └─ 불필요한 계정 존재 여부

[3단계: 그룹 점검]
   net localgroup Administrators
   └─ 관리자 그룹 구성원
   └─ 최소 권한 원칙 준수 여부

[4단계: 보안 정책 점검]
   secpol.msc
   ├─ 계정 정책
   │   ├─ 암호 정책 (길이, 복잡성, 유효기간)
   │   └─ 계정 잠금 정책 (임계값, 잠금 기간)
   └─ 로컬 정책
       └─ 보안 옵션 (계정명 변경 등)

[5단계: 서비스 점검]
   services.msc
   └─ 불필요한 서비스 실행 여부
   └─ Telnet, FTP 등 위험 서비스 확인

[6단계: 로그 점검]
   eventvwr.msc
   └─ 보안 로그 (4625 실패 로그인)
   └─ 시스템 로그 (오류, 경고)
```

### 계정 잠금 정책 동작 원리
```
[정책 설정]
임계값: 5회
잠금 기간: 60분
카운터 재설정: 60분

[시나리오]
14:00 → 로그인 실패 (1회) → 4625 이벤트
14:01 → 로그인 실패 (2회) → 4625 이벤트
14:02 → 로그인 실패 (3회) → 4625 이벤트
14:03 → 로그인 실패 (4회) → 4625 이벤트
14:04 → 로그인 실패 (5회) → 4625 이벤트
        ↓
      임계값 도달
        ↓
14:04 → 계정 잠김 → 4740 이벤트
        ↓
15:04 → 자동 해제 (60분 후) → 4767 이벤트

[카운터 재설정]
14:00 → 실패 (1회)
14:01 → 실패 (2회)
15:02 → (60분 경과) → 카운터 0으로 리셋
```

---

## 💡 실기 답안 작성 팁

### Tip 1: net accounts 명령어 옵션
```
✅ /minpwlen:8
✅ /maxpwage:90
❌ -minpwlen 8 (리눅스 스타일 X)
❌ /minpwlen=8 (= 사용 X)
```

### Tip 2: sc config 명령어 공백 주의
```
✅ sc config Telnet start= disabled
   (start= 다음에 반드시 공백!)

❌ sc config Telnet start=disabled
   (공백 없으면 오류 발생)
```

### Tip 3: GUI 도구 실행 명령어
```
secpol.msc   (s로 시작 - security)
lusrmgr.msc  (l로 시작 - local user)
services.msc (s로 시작 - services)
eventvwr.msc (e로 시작 - event viewer)
```

### Tip 4: 취약 판정 기준
```
Guest 계정: 활성화되어 있으면 취약
계정 잠금 임계값: 0 (사용 안 함)이면 취약
최소 암호 길이: 8자 미만이면 취약
최대 암호 사용 기간: 90일 초과 또는 무제한이면 취약
```

---

**✅ KISA Windows 취약점 점검 Top 10만 정확히 외우면 실기 Windows 보안 문제 80% 커버!**
