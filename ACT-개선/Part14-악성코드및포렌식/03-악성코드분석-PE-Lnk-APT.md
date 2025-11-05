# 악성코드 분석 - PE, Lnk, APT, 공격 기법

## 🔴 PE (Portable Executable) 구조 ★★★

### 문제 (2024년 기출)
```
Windows PE 파일 구조의 주요 구성요소는?
```

### ✅ 정답
```
PE (Portable Executable) 파일 구조

Windows 실행 파일 형식 (.exe, .dll, .sys)

주요 구성요소: ★★★

┌─────────────────────────────────┐
│ DOS Header                      │  ← MZ 시그니처 (0x4D5A)
├─────────────────────────────────┤
│ DOS Stub                        │  ← "This program cannot be run..."
├─────────────────────────────────┤
│ PE Header                       │  ← PE 시그니처 (0x50450000)
│  - Signature (PE\0\0)          │     "PE"
│  - File Header                  │
│  - Optional Header              │
├─────────────────────────────────┤
│ Section Headers                 │  ← 각 섹션 정보
├─────────────────────────────────┤
│ .text  (코드 섹션) ★★★        │  ← 실행 코드
├─────────────────────────────────┤
│ .data  (데이터 섹션)           │  ← 초기화된 데이터
├─────────────────────────────────┤
│ .rdata (읽기 전용 데이터)      │  ← Import/Export Table
├─────────────────────────────────┤
│ .bss   (미초기화 데이터)       │
├─────────────────────────────────┤
│ .rsrc  (리소스 섹션)           │  ← 아이콘, 문자열
└─────────────────────────────────┘

1. DOS Header (64 bytes)
   - e_magic: "MZ" (0x4D5A) ★★★
   - e_lfanew: PE Header 오프셋

2. PE Header
   - Signature: "PE\0\0" (0x50450000) ★★★
   - Machine: CPU 타입 (0x014C = x86, 0x8664 = x64)
   - NumberOfSections: 섹션 개수
   - TimeDateStamp: 컴파일 시간

3. Optional Header (중요!)
   - Magic: 0x010B (32bit), 0x020B (64bit)
   - AddressOfEntryPoint (EP) ★★★
     → 프로그램 시작 주소 (OEP)
   - ImageBase: 메모리 로드 주소
   - SizeOfImage: 메모리 크기
   - DataDirectory: Import/Export/Resource 정보

4. Section Headers
   각 섹션의 정보:
   - Name: 섹션 이름
   - VirtualAddress: 메모리 주소
   - SizeOfRawData: 파일 크기
   - PointerToRawData: 파일 오프셋
   - Characteristics: 속성 (실행/읽기/쓰기)

5. Import Address Table (IAT) ★★★
   - 사용하는 DLL 및 함수 목록
   - kernel32.dll: CreateFile, ReadFile
   - user32.dll: MessageBox
   - ws2_32.dll: socket, connect (네트워크)

   악성코드 분석 시 중요:
   - RegSetValue → 레지스트리 변경
   - CreateProcess → 프로세스 생성
   - URLDownloadToFile → 파일 다운로드 ★★

6. Export Address Table (EAT)
   - DLL이 제공하는 함수 목록

악성코드 분석 포인트:

1. EP (Entry Point) 확인 ★★★
   - 패킹 시 EP가 비정상 섹션
   - 정상: .text 섹션
   - 의심: UPX0, UPX1, .aspack

2. IAT 확인 ★★★
   의심 함수:
   - URLDownloadToFile (파일 다운로드)
   - CreateRemoteThread (인젝션)
   - WriteProcessMemory (인젝션)
   - InternetOpen (네트워크)
   - RegSetValue (레지스트리)

3. 섹션 권한
   - .text: 실행 권한 (정상)
   - .data: 쓰기 권한 (정상)
   - 의심: 데이터 섹션에 실행 권한

4. 리소스 확인
   - 아이콘, 버전 정보
   - 숨겨진 데이터 (악성코드 본체)

도구:
- PEview ★★
- CFF Explorer
- PE-bear
- PEiD (패킹 탐지)
```

---

## 🔴 .Lnk 파일 (바로가기) ★★

### 문제 (2024년 기출)
```
.Lnk 파일을 이용한 공격은?
```

### ✅ 정답
```
.Lnk 파일 (Shortcut File)
= Windows 바로가기 파일

악용 사례: ★★★

1. Stuxnet 웜 (2010년) ★★★
   - .Lnk 파일 취약점 악용 (CVE-2010-2568)
   - USB를 통해 전파
   - 이란 원자력 시설 공격

   공격 흐름:
   1) 악성 .lnk 파일이 USB에 저장
   2) 탐색기에서 USB 폴더 열기
   3) .lnk 파일 아이콘 렌더링 시 자동 실행 ★★★
   4) 악성코드 실행

2. 피싱 공격
   - .lnk 파일을 이메일 첨부
   - 실제 타겟: 악성 스크립트/실행 파일

   예시:
   파일명: 보고서.pdf.lnk
   실제 타겟: C:\Windows\System32\cmd.exe /c powershell -enc [base64]

3. LNK 스푸핑
   - 아이콘을 정상 파일처럼 위장
   - 확장자 숨김 (Windows 기본 설정)

.Lnk 파일 구조:

HeaderSize (76 bytes)
LinkCLSID
LinkFlags
FileAttributes
CreationTime, AccessTime, WriteTime
FileSize
IconIndex
ShowCommand
HotKey
TargetIDList (실제 타겟 경로) ★★★
...

분석 도구:
- LECmd (Eric Zimmerman) ★★
- LnkParse
- Windows Properties (우클릭 → 속성)

공격 예시:

정상 .lnk:
타겟: C:\Program Files\app.exe

악성 .lnk:
타겟: cmd.exe /c start malware.exe && start document.pdf
                    ↑                  ↑
               악성코드 실행       정상 문서 열기 (위장)

방어:

1. 확장자 표시 ★★
   폴더 옵션 → "알려진 파일 형식의 확장자 숨기기" 해제

2. .lnk 파일 실행 주의
   - 출처 불명 파일 실행 금지
   - 이메일 첨부 .lnk 차단

3. USB 자동 실행 비활성화 ★★★
   AutoRun 레지스트리 설정

4. 패치 적용
   - MS10-046 (CVE-2010-2568)

5. AppLocker / SRP
   - .lnk 실행 제한
```

---

## 🔴 랜섬웨어 ★★★

### 문제 (2024년 기출)
```
랜섬웨어의 동작 원리와 대응 방법은?
```

### ✅ 정답
```
랜섬웨어 (Ransomware)

정의:
파일을 암호화하고 금전(Ransom)을 요구하는 악성코드

동작 원리: ★★★

1. 감염
   - 피싱 이메일 (첨부 파일)
   - 취약한 RDP (3389 포트) ★★★
   - Exploit Kit (Drive-by Download)
   - SMB 취약점 (EternalBlue) ★★★

2. 암호화
   - 사용자 파일 검색 (.doc, .pdf, .jpg 등)
   - 대칭키 암호화 (AES) ★★★
   - 대칭키를 공격자 공개키로 암호화 (RSA)
   - 원본 파일 삭제 (복구 불가)

3. 금전 요구
   - 바탕화면에 협박 메시지
   - 비트코인으로 결제 요구
   - 기한 내 미지불 시 파일 영구 삭제

4. 복호화 (결제 시)
   - 공격자가 개인키로 대칭키 복호화
   - 복호화 도구 제공 (보장 안 됨!)

주요 랜섬웨어:

1. WannaCry (2017년) ★★★
   - SMB 취약점 (MS17-010, EternalBlue)
   - 전 세계 30만대 감염
   - 445 포트 공격

2. Petya / NotPetya (2017년)
   - MBR (Master Boot Record) 암호화
   - 부팅 자체 불가

3. Locky (2016년)
   - 이메일 첨부 매크로

4. Ryuk (2019년~)
   - 기업 타겟팅
   - 네트워크 전파

5. REvil / Sodinokibi
   - RaaS (Ransomware as a Service)

암호화 방식:

파일 암호화:
1. AES 대칭키 생성 (랜덤) ★★★
2. 파일을 AES로 암호화
3. AES 키를 공격자 RSA 공개키로 암호화
4. 암호화된 AES 키를 파일에 저장

→ 공격자만 RSA 개인키 보유 ★★★
→ 복호화 불가능!

방어: ★★★

1. 백업 ★★★
   - 정기 백업 (3-2-1 규칙)
   - 오프라인 백업 (네트워크 분리)
   - 백업 테스트

2. 패치 적용 ★★★
   - Windows 업데이트
   - MS17-010 (EternalBlue)

3. SMB 포트 차단 ★★★
   - 445, 139 포트 차단
   - SMBv1 비활성화

4. RDP 보안 ★★
   - 강력한 비밀번호
   - 2FA 인증
   - VPN 연동
   - 3389 포트 변경/차단

5. 이메일 보안
   - 매크로 비활성화 ★★
   - 첨부 파일 주의

6. EDR / 백신
   - 행위 기반 탐지
   - Ransomware Protection

7. 권한 관리
   - 최소 권한 원칙
   - 관리자 권한 최소화

대응 (감염 시):

1. 즉시 네트워크 차단 ★★★
   - 전파 방지

2. 시스템 격리
   - 추가 감염 차단

3. 복호화 도구 검색
   - No More Ransom Project
   - Kaspersky, Avast 등

4. 결제 금지 권고 ★★
   - 복호화 보장 없음
   - 범죄 자금 제공

5. 백업 복구
   - 오프라인 백업 사용

6. 포렌식 분석
   - 침입 경로 분석
   - 재발 방지
```

---

## 🔴 APT 공격 벡터 ★★★

### 문제 (2024년 기출)
```
APT 공격의 주요 공격 경로는?
```

### ✅ 정답
```
APT (Advanced Persistent Threat)
= 지능형 지속 위협

특징:
- 특정 조직 타겟팅 ★★★
- 장기간 지속
- 고도화된 기법
- 국가 배후 (일반적)

주요 공격 벡터: ★★★

1. 스피어 피싱 (Spear Phishing) ★★★
   - 특정 개인 타겟팅
   - 맞춤형 이메일
   - 악성 첨부 파일 또는 링크

   예시:
   제목: [긴급] 임원 회의 자료
   첨부: 회의자료.doc (악성 매크로)

2. 워터링 홀 (Watering Hole) ★★★
   - 타겟이 자주 방문하는 웹사이트 감염
   - Drive-by Download
   - 제로데이 취약점 악용

3. 공급망 공격 (Supply Chain) ★★
   - 신뢰받는 소프트웨어 감염
   - 업데이트 서버 해킹

   예시:
   - SolarWinds (2020년)
   - CCleaner (2017년)

4. 0-day 취약점 악용 ★★★
   - 알려지지 않은 취약점
   - 패치 없음
   - 탐지 어려움

5. 소셜 엔지니어링
   - 내부자 포섭
   - 전화, 대면 접촉

6. 물리적 침투
   - USB 드롭 (주차장에 악성 USB)
   - 건물 무단 침입

APT 공격 단계: (Kill Chain)

1. 정찰 (Reconnaissance)
   - 타겟 정보 수집
   - 소셜 미디어 분석

2. 무기화 (Weaponization)
   - 악성코드 제작
   - 익스플로잇 준비

3. 전달 (Delivery)
   - 피싱 이메일
   - 워터링 홀

4. 악용 (Exploitation)
   - 취약점 공격
   - 초기 감염

5. 설치 (Installation)
   - 백도어 설치
   - 지속성 확보 (레지스트리, 서비스)

6. C&C 통신 (Command & Control)
   - 공격자 서버 연결
   - 명령 수신

7. 목표 달성 (Actions on Objectives)
   - 데이터 탈취
   - 시스템 파괴
   - 스파이 활동

주요 APT 그룹:

1. Lazarus (북한)
   - 소니 픽처스 (2014년)
   - WannaCry (2017년)

2. APT28 / Fancy Bear (러시아)
   - DNC 해킹 (2016년)

3. APT1 (중국)
   - 산업 스파이

4. Equation Group (미국 NSA 추정)
   - Stuxnet

방어: ★★★

1. 다층 방어 (Defense in Depth)
   - 여러 보안 계층

2. 제로 트러스트 (Zero Trust)
   - 모든 접근 검증
   - 내부 네트워크도 신뢰 안 함

3. EDR / XDR
   - 엔드포인트 탐지 및 대응
   - 행위 기반 분석

4. 위협 인텔리전스
   - IoC (Indicators of Compromise) 공유
   - MITRE ATT&CK 프레임워크

5. 네트워크 세그먼테이션
   - 핵심 자산 격리

6. 로그 분석 및 SIEM
   - 이상 징후 탐지

7. 보안 인식 교육 ★★
   - 피싱 시뮬레이션
   - 정기 교육
```

---

## 🔴 Hoax (허위 정보) ★

### 문제 (2024년 기출)
```
Hoax란?
```

### ✅ 정답
```
Hoax = 허위 정보, 거짓 경고

정의:
실제 위협이 아니지만
바이러스나 보안 위협으로 위장한 허위 정보

특징:
- 악성코드 아님
- 사회 공학 기법
- 공포 유발
- 빠른 전파 (이메일, SNS)

유형:

1. 바이러스 허위 경고 ★★
   "긴급! 새로운 바이러스가 발견되었습니다.
    system32 폴더의 xxx.exe를 삭제하세요!"

   → 실제로는 정상 시스템 파일
   → 사용자가 직접 시스템 파괴

2. 체인 메일 (Chain Letter)
   "이 메일을 10명에게 전달하지 않으면..."
   - 행운/불행 협박

3. 피싱 사칭
   "당신의 계정이 해킹되었습니다. 링크 클릭!"
   - 실제 해킹 아님

4. 공포 마케팅
   "무료 백신 다운로드!"
   - 실제로는 가짜 백신 (Scareware)

피해:

1. 시스템 파일 삭제
   - 사용자가 직접 파괴

2. 네트워크 과부하
   - 이메일 대량 전송

3. 시간/자원 낭비
   - 대응에 소요되는 비용

4. 신뢰 저하
   - 실제 경고 무시

대응:

1. 검증 ★★★
   - 공식 보안 사이트 확인
   - Snopes.com (허위 정보 검증 사이트)

2. 전파 중단
   - 이메일 전달 금지

3. 사용자 교육
   - 의심스러운 경고 무시
   - 공식 채널 확인

4. 삭제 금지
   - system32 파일 함부로 삭제 금지
```

---

## 🔴 DoS 공격 유형 ★★★

### 문제 (2024년 기출)
```
DoS 공격 유형과 방어 방법은?
```

### ✅ 정답
```
DoS (Denial of Service)
= 서비스 거부 공격

주요 공격 유형:

1. SYN Flooding ★★★
   - TCP 3-way Handshake 악용
   - SYN 패킷만 대량 전송
   - 서버의 연결 대기 큐 고갈

   방어:
   - SYN Cookie ★★★
   - 방화벽 SYN Proxy

2. UDP Flooding ★★
   - UDP 패킷 대량 전송
   - 대역폭 소진

3. ICMP Flooding (Ping Flood)
   - ICMP Echo Request 대량 전송

4. HTTP Flooding ★★
   - HTTP 요청 대량 전송
   - 응용 계층 공격

5. Slowloris ★★
   - HTTP 요청을 천천히 전송
   - 연결 유지로 리소스 고갈

   예시:
   GET / HTTP/1.1
   Host: target.com
   (계속 헤더 전송... 완료 안 함)

6. Teardrop Attack
   - IP 단편화 패킷 조작
   - 재조립 시 오버플로우

7. Land Attack
   - 출발지 IP = 목적지 IP
   - 자기 자신에게 패킷 전송

8. Smurf Attack ★★
   - ICMP Echo를 브로드캐스트
   - 출발지 IP를 피해자로 위조
   - 증폭 공격

DDoS (Distributed DoS) ★★★

- 여러 좀비 PC(봇넷)에서 동시 공격
- 공격원 추적 어려움
- 막대한 트래픽

구조:
┌──────────┐
│ 공격자   │
└─────┬────┘
      │
┌─────▼──────┐
│ C&C 서버   │
└──┬──┬──┬──┘
   │  │  │
┌──▼─▼──▼──┐
│ 봇넷      │ (수천~수만 대)
└──┬──┬──┬─┘
   │  │  │
   ▼  ▼  ▼
┌──────────┐
│ 피해 서버│
└──────────┘

주요 DDoS:
- Mirai 봇넷 (2016년)
  → IoT 기기 감염
  → DNS 서비스 (Dyn) 마비

방어: ★★★

1. 방화벽 / IPS
   - Rate Limiting (속도 제한)
   - 비정상 패킷 차단

2. SYN Cookie ★★★
   - SYN Flood 방어
   - 연결 테이블 사용 안 함

3. Anti-DDoS 서비스
   - Cloudflare
   - Akamai
   - AWS Shield

4. 대역폭 확장
   - 여유 대역폭 확보

5. CDN (Content Delivery Network)
   - 트래픽 분산

6. 블랙홀 라우팅
   - 공격 트래픽을 null0로 전송

7. 탐지
   - Netflow 분석
   - 비정상 트래픽 패턴 탐지
```

---

## 🔴 Pharming ★★

### 문제 (2024년 기출)
```
Pharming 공격이란?
```

### ✅ 정답
```
Pharming = DNS 조작으로 가짜 사이트 유도

vs Phishing:
┌──────────┬──────────────┬──────────────┐
│   구분   │  Phishing    │  Pharming    │
├──────────┼──────────────┼──────────────┤
│ 방법     │ 가짜 링크   │ DNS 조작 ★  │
├──────────┼──────────────┼──────────────┤
│ 사용자   │ 클릭 필요   │ 자동 유도 ★ │
│ 행위     │              │              │
├──────────┼──────────────┼──────────────┤
│ 위험도   │ 보통         │ 높음 ★★    │
└──────────┴──────────────┴──────────────┘

공격 방법:

1. Hosts 파일 조작 ★★★
   위치:
   - Windows: C:\Windows\System32\drivers\etc\hosts
   - Linux: /etc/hosts

   정상:
   127.0.0.1  localhost

   악성 코드가 추가:
   1.2.3.4  bank.com    ← 가짜 IP
   1.2.3.4  naver.com

   결과:
   사용자가 bank.com 입력
   → 1.2.3.4 (공격자 서버)로 연결 ★★★
   → 가짜 은행 사이트

2. DNS Cache Poisoning ★★★
   - DNS 캐시 서버 해킹
   - 잘못된 DNS 응답 주입

   정상:
   bank.com → 10.0.0.1 (진짜)

   공격 후:
   bank.com → 1.2.3.4 (가짜)

3. DNS 서버 변조
   - 라우터 DNS 설정 변경
   - 공격자 DNS 서버로 설정

4. Man-in-the-Middle
   - ARP Spoofing으로 DNS 응답 조작

피해:

- 정상 URL 입력해도 가짜 사이트
- 주소 표시줄에도 정상 URL ★★
- SSL 인증서 경고만이 단서
- 개인정보/금융정보 탈취

방어:

1. Hosts 파일 보호 ★★
   - 읽기 전용 설정
   - 주기적 확인

   확인:
   notepad C:\Windows\System32\drivers\etc\hosts

2. DNS 보안 (DNSSEC) ★★★
   - DNS 응답 서명 검증
   - 위조 방지

3. 공인 DNS 사용
   - Google DNS (8.8.8.8)
   - Cloudflare DNS (1.1.1.1)
   - 신뢰할 수 있는 DNS

4. HTTPS 확인 ★★★
   - SSL 인증서 검증
   - 주소 표시줄 자물쇠 아이콘

5. 백신 / 보안 소프트웨어
   - Hosts 파일 모니터링

6. 라우터 관리
   - 관리자 비밀번호 변경
   - 펌웨어 업데이트
```

---

## 🔴 Embedded 기기 분석 ★

### 문제 (2024년 기출)
```
임베디드 기기 악성코드 분석 방법은?
```

### ✅ 정답
```
Embedded Device (임베디드 기기) 분석

대상:
- IoT 기기 (IP 카메라, 공유기)
- 스마트 가전
- SCADA 시스템 ★★
- 산업 제어 시스템 (ICS)

특징:
- ARM, MIPS 등 CPU (x86 아님)
- Linux 기반 (임베디드 리눅스)
- 제한된 리소스
- 업데이트 어려움

분석 방법:

1. 펌웨어 추출
   - UART 포트 연결 (시리얼 통신)
   - JTAG 디버깅
   - 펌웨어 다운로드 (제조사 웹)
   - SPI Flash 덤프

2. 펌웨어 분석
   - Binwalk ★★
     binwalk firmware.bin
     → 파일 시스템, 압축 파일 탐지

   - 추출:
     binwalk -e firmware.bin
     → 파일 시스템 추출

3. 파일 시스템 분석
   - 설정 파일 (/etc)
   - 웹 서버 (lighttpd, httpd)
   - 기본 패스워드 확인

4. 바이너리 분석
   - IDA Pro (ARM 디스어셈블리)
   - Ghidra
   - QEMU (에뮬레이션) ★★

5. 네트워크 분석
   - 기기를 격리 네트워크에 연결
   - Wireshark로 통신 분석
   - 백도어 포트 확인

취약점:

1. 기본 패스워드 ★★★
   - admin / admin
   - root / root
   - 변경 안 함

2. 백도어
   - 숨겨진 계정
   - 디버그 포트 (Telnet 23)

3. 웹 인터페이스 취약점
   - 인증 우회
   - Command Injection

4. 펌웨어 업데이트 미제공
   - 패치 불가능

주요 사례:

- Mirai 봇넷 ★★★
  → IoT 기기 감염 (기본 패스워드)
  → DDoS 공격

- VPNFilter
  → 라우터 감염

방어:

- 기본 패스워드 변경 ★★★
- 불필요한 서비스 비활성화
- 펌웨어 업데이트
- 네트워크 세그먼테이션
```

---

## 🔴 Landing Page 분석 ★

### 문제 (2024년 기출)
```
악성코드 유포지(Landing Page) 분석 방법은?
```

### ✅ 정답
```
Landing Page (악성코드 유포지)

정의:
악성코드를 유포하는 웹 페이지

특징:
- Exploit Kit 호스팅
- 난독화된 JavaScript
- 리다이렉션 체인
- 빠른 도메인 변경

분석 방법:

1. 안전한 환경 구축 ★★★
   - VM (스냅샷)
   - 격리 네트워크
   - 백신 비활성화 (분석 목적)

2. 브라우저 모니터링
   - Process Monitor
   - HTTP 디버거 (Fiddler)
   - 브라우저 개발자 도구

3. JavaScript 난독화 해제
   - jsbeautifier.org
   - de4js

4. 네트워크 분석 ★★
   - Wireshark
   - 다운로드된 파일 확인
   - C&C 서버 IP 추출

5. Exploit Kit 식별
   - Angler, RIG 등
   - CVE 번호 확인

IoC (Indicators of Compromise) 추출:
- 악성 도메인
- IP 주소
- 파일 해시

방어:
- 브라우저 업데이트
- NoScript
- IPS 시그니처
```

---

## 🎯 악성코드 분석 핵심 암기 카드

### 카드 1: PE 구조
```
MZ 시그니처 (DOS Header)
PE 시그니처 (PE Header)
EP (Entry Point) ★★★
IAT (Import Table) - 사용 함수 확인 ★★
```

### 카드 2: .Lnk
```
Stuxnet 웜 (USB 전파) ★★★
아이콘 렌더링 시 자동 실행
확장자 표시 필수
```

### 카드 3: 랜섬웨어
```
AES 파일 암호화 + RSA 키 암호화 ★★★
WannaCry (EternalBlue, 445 포트)
백업이 최선의 방어!
```

### 카드 4: APT
```
스피어 피싱, 워터링 홀 ★★★
Kill Chain 7단계
EDR, 제로 트러스트
```

### 카드 5: DoS
```
SYN Flooding → SYN Cookie 방어 ★★★
DDoS (봇넷)
Anti-DDoS 서비스, CDN
```

### 카드 6: Pharming
```
Hosts 파일 조작 ★★★
DNS Cache Poisoning
DNSSEC, HTTPS 확인
```

---

## 🔥 실기 시험 최빈출 악성코드 개념

```
1위: PE 구조 (EP, IAT)              ← 2024년 출제! ★★★
2위: 랜섬웨어 (WannaCry, 445)       ← 2024년 출제! ★★★
3위: APT 공격 벡터                   ← 2024년 출제!
4위: DoS/DDoS (SYN Flooding)        ← 2024년 출제!
5위: Pharming (Hosts 파일)          ← 2024년 출제!
6위: .Lnk 파일 (Stuxnet)            ← 2024년 출제!
7위: Embedded 기기 (Mirai)          ← 2024년 출제!
```

**✅ 랜섬웨어는 445 포트 차단과 백업이 핵심 방어!**
**✅ PE의 IAT는 악성코드 행위 파악의 시작점!**
**✅ Pharming은 Hosts 파일 보호가 최우선!**
