# 정보보안기사 실기 완전 정리 - 전체 목차 및 세부 개념

## Part 1. 정보보호 관리체계 및 위험관리

### 1.1 정보보호 관리체계 (ISMS-P)

#### ISMS-P 인증체계
- 3개 영역: 관리체계 수립 및 운영, 보호대책 요구사항, 개인정보 처리 단계별 요구사항
- 102개 세부 항목
- 관리적 요구사항 절차: 정보보호정책 수립→경영진 책임 및 조직 구성→위험관리→정보보호대책 구현→사후관리

#### 정보보호 조직
- CISO (Chief Information Security Officer): 정보보호 최고책임자
- SOC (Security Operation Center): 보안관제센터
- CERT (Computer Emergency Response Team): 침해사고 대응팀

### 1.2 접근통제 모델

#### 접근통제 모델 분류
- **DAC** (Discretionary Access Control): 임의적 접근통제, 소유자가 권한 결정
- **MAC** (Mandatory Access Control): 강제적 접근통제, 보안등급 기반
- **RBAC** (Role-Based Access Control): 역할 기반 접근통제

#### 보안 모델
- **Bell-LaPadula 모델**: 기밀성 중심
  - No Read Up (상위 등급 읽기 금지)
  - No Write Down (하위 등급 쓰기 금지)
- **Biba 모델**: 무결성 중심
  - No Write Up (상위 등급 쓰기 금지)
  - No Read Down (하위 등급 읽기 금지)
- **Clark-Wilson 모델**: 무결성, 상업용 환경
- **Chinese Wall 모델**: 이해충돌 방지

#### 기본 보안 원칙
- **보안 3요소 (CIA Triad)**:
  - 기밀성 (Confidentiality): 인가된 사용자만 접근
  - 무결성 (Integrity): 인가된 방법으로만 변경
  - 가용성 (Availability): 필요시 접근 가능
- **AAA**:
  - 인증 (Authentication): 신원 확인
  - 인가 (Authorization): 권한 부여
  - 계정관리 (Accounting): 사용 추적
- **부인방지** (Non-repudiation): 디지털 서명
- **최소권한 원칙** (Principle of Least Privilege)
- **직무분리** (Separation of Duties)
- **다층방어** (Defense in Depth)
- **Fail-Safe**: 장애시 안전한 상태로
- **Fail-Secure**: 장애시 보안 우선

### 1.3 위험관리

#### 위험평가 방법론
- 베이스라인 접근법: 체크리스트 기반 표준화된 보안대책
- 비정형화된 접근법: 경험자 지식 활용
- 상세 위험분석: 자산·위협·취약점 단계별 분석
- 복합접근법: 위 세가지 혼합

#### 위험분석 구성요소
- 자산 식별 및 중요도 산정 (CIA 기준)
- 위협 식별 및 평가
- 취약점 분석
- 위험도 = 자산가치 × 위협 × 취약점

#### 위험대응 전략
- 위험수용 (Risk Accept): 일정 수준 이하 위험 수용
- 위험회피 (Risk Avoid): 위험 프로세스 포기
- 위험전가 (Risk Transfer): 보험, 외주로 제3자 이전
- 위험감소/완화 (Risk Mitigate): 보호대책으로 위험 낮춤

### 1.4 침해사고 대응
- 사이버위기 경보 단계: 관심→주의→경계→심각
- 침해사고 대응 7단계: 사고 전 준비→사고 탐지→초기 대응→대응 전략 체계화→사고 조사→보고서 작성→해결
- 사이버 킬체인: 7단계 공격 흐름 분석
- MITRE ATT&CK: 14단계 확장 모델

## Part 2. 시스템 보안

### 2.1 Linux 시스템 보안

#### 주요 시스템 파일
- `/etc/passwd`: 사용자 계정 정보
- `/etc/shadow`: 암호화된 패스워드 (root만 열람 가능)
- `/etc/group`: 그룹 정보
- `/etc/login.defs`: 패스워드 정책 (PASS_MIN_LEN 8)
- `/etc/hosts.allow`, `/etc/hosts.deny`: tcpwrapper 설정

#### 로그 파일 관리
- `utmp`: 현재 로그인 사용자 상태
- `wtmp`: 로그인/로그아웃/재부팅 정보
- `btmp`: 5회 이상 로그인 실패 기록
- `lastlog`: 마지막 로그인 정보
- `acct/pacct`: 프로세스 계정 정보
- `/var/log/message`: 시스템 로그 (프로세스명, PID, 메시지)

#### 로그 관리 설정
- logrotate 설정: weekly(주단위), create(새 파일 생성), compress(압축)
- 로그 순환 및 보관 정책

#### 주요 명령어

**로그 관련**
- `lastcomm`: pacct 로그 확인
- `lastb`: 실패한 로그인 기록
- `lsof`: 열린 파일 및 프로세스 확인
- `who`, `last`, `lastlog`: 로그인 관련 명령어

**프로세스 관리**
- `ps -ef`: 모든 프로세스 조회
- `top`: 실시간 프로세스 모니터링
- `kill -9 PID`: 프로세스 강제 종료
- `killall`: 프로세스명으로 종료

**네트워크 명령어**
- `netstat -ano`: 네트워크 연결 상태
- `tcpdump`: 패킷 캡처
- `ifconfig`, `ip addr`: 네트워크 인터페이스 설정
- `iptables`: 방화벽 설정

**파일 검색/처리**
- `find / -name "*.conf"`: 파일 검색
- `grep "pattern" file`: 패턴 검색
- `awk`, `sed`: 텍스트 처리

**시스템 관리**
- `cron/crontab`: 작업 스케줄링
- `service`, `systemctl`: 서비스 관리

#### 파일 권한 관리
- **chmod**: 권한 변경 (r=4, w=2, x=1)
  - 예: chmod 755 file (rwxr-xr-x)
- **chown**: 소유자 변경
- **chgrp**: 그룹 변경
- **umask**: 기본 권한 마스크 (022 → 755)
- **특수 권한**:
  - SetUID (4000): 소유자 권한으로 실행
  - SetGID (2000): 그룹 권한으로 실행
  - Sticky bit (1000): 소유자만 삭제 가능 (/tmp)

#### 주요 설정 파일 추가
- `/etc/ssh/sshd_config`: SSH 서버 설정
  - PermitRootLogin no
  - PasswordAuthentication no
- `/etc/resolv.conf`: DNS 서버 설정
- `/etc/fstab`: 파일시스템 마운트 정보

#### 보안 설정
- PAM 모듈: auth(인증), account(권한확인), session(세션관리), password(패스워드변경)
- xinetd 서비스: no_access, only_from, instances, access_time 설정
- Dynamic Linking: PLT(Procedure Linkage Table), GOT(Global Offset Table)
- 64비트 레지스터: RSI, RDX, RCX (함수 파라미터 저장)

### 2.2 Windows 시스템 보안

#### 이벤트 로그 관리
- 이벤트 뷰어 설정 및 관리
- 로그 용량 계산: 500바이트 × 1,000건 × 30일 = 15MB
- 보안 이벤트 ID 관리

#### 로그 파일 경로
- IIS 로그: `C:\Windows\inetpub\logs\Logfiles\W3SVC1`
- HTTPERR 로그: `C:\Windows\System32\Logfiles\HTTPERR`
- DHCP 로그: `C:\Windows\System32\Logfiles\DHCP`

#### 파일시스템 및 암호화
- NTFS, FAT32, exFAT
- BitLocker: TPM 기반 볼륨 암호화

#### PE 구조
- `.text`: 코드 영역
- `.data`: 전역변수, 상수 영역
- `.idata`: DLL import 함수 정보

#### DNS 설정
- Zone 설정
- 리소스 레코드 설정

#### 레지스트리 및 주요 파일
- **레지스트리 하이브**:
  - HKEY_LOCAL_MACHINE (HKLM): 시스템 전체 설정
  - HKEY_CURRENT_USER (HKCU): 사용자별 설정
- **SAM 파일**: `C:\Windows\System32\config\SAM` (계정정보 저장)
- **Active Directory**: 중앙 집중식 인증 및 권한 관리
- **그룹정책(GPO)**: 도메인 정책 관리

#### Windows 명령어
- `net user`: 사용자 계정 관리
- `net localgroup`: 로컬 그룹 관리
- `netstat -ano`: 네트워크 연결 상태
- `tasklist`, `taskkill`: 프로세스 관리

#### 기타
- Unix 계정 파일: `/etc/default/login`, `/etc/security/user`, `/etc/securetty`

### 2.3 메모리 보안

#### 공격 기법
- Pass the Hash: NTLM/LanMan 해시 탈취
- mimikatz 도구 대응
- 메모리 덤프 분석

#### 메모리 보호 기법
- **ASLR** (Address Space Layout Randomization): 메모리 주소 랜덤화
- **DEP/NX bit** (Data Execution Prevention): 데이터 영역 실행 방지
- **Stack Canary**: 스택 오버플로우 탐지 값
- **PIE** (Position Independent Executable): 위치 독립 실행

#### 메모리 공격 기법
- **Buffer Overflow**: 버퍼 경계 초과
  - Stack Overflow: 스택 영역 오버플로우
  - Heap Overflow: 힙 영역 오버플로우
- **ROP** (Return Oriented Programming): DEP 우회 기법
- **Format String Attack**: printf 포맷 문자열 악용
- **Use After Free**: 해제된 메모리 재사용

#### 익스플로잇 기법
- 쉘코드: 기계어로 구성된 exploit 본체
- NOP sled: 0x90 (No Operation)
- Jump ESP: ESP→EIP 이동

## Part 3. 네트워크 보안

### 3.1 네트워크 프로토콜 기초

#### OSI 7계층 & TCP/IP 4계층
- **OSI 7계층**:
  1. Physical (물리): 비트 전송
  2. Data Link (데이터링크): 프레임, MAC 주소
  3. Network (네트워크): 패킷, IP 주소
  4. Transport (전송): 세그먼트, 포트
  5. Session (세션): 세션 관리
  6. Presentation (표현): 데이터 형식 변환
  7. Application (응용): 사용자 인터페이스

- **TCP/IP 4계층**:
  1. Network Access (네트워크 접근): OSI 1-2계층
  2. Internet (인터넷): OSI 3계층
  3. Transport (전송): OSI 4계층
  4. Application (응용): OSI 5-7계층

#### TCP 연결 관리
- **3-way handshake** (연결 수립):
  1. SYN →
  2. ← SYN+ACK
  3. ACK →
- **4-way handshake** (연결 종료):
  1. FIN →
  2. ← ACK
  3. ← FIN
  4. ACK →

#### 주요 포트번호
- **FTP**: 20(data), 21(control)
- **SSH**: 22
- **Telnet**: 23
- **SMTP**: 25
- **DNS**: 53
- **HTTP**: 80
- **POP3**: 110
- **IMAP**: 143
- **SNMP**: 161(agent), 162(manager)
- **LDAP**: 389
- **HTTPS**: 443
- **SSDP**: 1900

#### 서브넷팅
- **CIDR 표기법**: /24 (255.255.255.0), /16 (255.255.0.0)
- **서브넷 마스크**: 네트워크 부분과 호스트 부분 구분
- **사용 가능 호스트 수**: 2^n - 2 (네트워크주소, 브로드캐스트 제외)

#### 주소 변환 프로토콜
- ARP: IP→MAC 주소 변환 (브로드캐스트 FF:FF:FF:FF:FF:FF)
- RARP: MAC→IP 주소 변환 (Reverse ARP)

#### 네트워크 서비스
- DHCP: 동적 IP 할당 프로토콜
- DNS: 도메인 이름 시스템
  - UDP/TCP 53번 포트
  - DNS 캐시 및 TTL
  - Recursive DNS vs Authoritative DNS
  - Zone Transfer (AXFR) 설정: allow-transfer
- NAT/PAT: 네트워크 주소 변환

### 3.2 네트워크 공격 기법

#### DoS/DDoS 공격
- SYN Flooding: TCP 3-way handshake 악용
- Smurf Attack: ICMP echo request 증폭 공격
- Land Attack: 출발지=목적지 IP 동일 설정
- Teardrop: Fragment offset 중첩
- Ping of Death: 패킷 크기 초과
- SSDP DRDoS: 1900번 포트 IoT 공격
- Slowloris (Slow HTTP Header DoS): HTTP 헤더 CRLF 조작

#### 스캔 공격

**nmap 스캔 기법**
- **-sT**: TCP Connect 스캔 (전체 3-way handshake)
- **-sS**: SYN 스캔 (Stealth 스캔, 반개방)
- **-sU**: UDP 스캔
- **-sN**: Null 스캔 (플래그 없음)
- **-sF**: FIN 스캔 (FIN 플래그만)
- **-sX**: XMAS 스캔 (FIN+PSH+URG 플래그)
- **-sA**: ACK 스캔 (방화벽 탐지)
- **-O**: OS 탐지
- **-D**: Decoy 스캔 (위장 IP 사용)

#### 스푸핑/하이재킹
- 세션 하이재킹: 연결된 세션 가로채기
- ARP Spoofing/Poisoning
- DNS 캐시 포이즈닝
- Switch Jamming: MAC 테이블 오버플로우

#### 기타 공격
- HTTP Request Smuggling: Content-Length/Transfer-Encoding 조작
- 크리덴셜 스터핑 (Credential Stuffing): 탈취 계정정보 재사용
- Directed Broadcast: 브로드캐스트 증폭

### 3.3 네트워크 보안 프로토콜

#### 암호화 프로토콜
- IPSec: 네트워크 계층
  - AH (Authentication Header): 무결성, 인증
  - ESP (Encapsulating Security Payload): 암호화, 기밀성
  - 터널모드 vs 전송모드
- TLS/SSL: 전송 계층 보안
- VPN 프로토콜

#### 인증 프로토콜
- RADIUS, TACACS+
- Kerberos
- EAP (Extensible Authentication Protocol)

### 3.4 네트워크 장비 보안

#### 방화벽 종류
- 스크리닝 라우터 (패킷필터링)
- 베스천 호스트 (프록시)
- 듀얼홈 게이트웨이
- 스크린드 서브넷 (DMZ)

#### VLAN 설정
- 기반 VLAN: 포트, MAC, 네트워크 주소, 프로토콜
- **802.1Q**: VLAN 태깅 표준
- 설정 명령: static, dynamic, show vlan

#### 스위치 보안
- **Spanning Tree Protocol (STP)**: 루프 방지
- **Port Security**: MAC 주소 제한
- **DHCP Snooping**: 비인가 DHCP 차단

#### 라우팅 프로토콜
- **RIP** (Routing Information Protocol): 거리벡터, hop count
- **OSPF** (Open Shortest Path First): 링크상태, 최단경로
- **BGP** (Border Gateway Protocol): 외부 라우팅

#### SNMP
- 매니저-에이전트 구조 네트워크 관리
- UDP 161(agent), 162(manager)
- v1/v2c: 평문 전송, v3: 암호화 지원

### 3.5 무선 보안

#### 무선랜 보안
- WPA2: EAP 인증 + AES-CCMP 암호화
- WPA3: 최신 보안 표준
- CSMA/CA: 충돌 회피 프로토콜
- RTS/CTS: 타임아웃 설정

## Part 4. 웹 애플리케이션 보안

### 4.1 웹 취약점 (OWASP Top 10)

#### 인젝션 공격
- SQL Injection: 쿼리 조작
  - 우회기법: 주석처리, OR 조건문
  - 대응: Prepared Statement, 입력값 검증, 최소권한 원칙
- Command Injection: OS 명령어 삽입
- LDAP/XML Injection
- include/require 취약점: allow_url_fopen=off 설정

#### XSS (Cross-Site Scripting)
- Reflected XSS: 즉시 반사형
- Stored XSS: 저장형
- DOM-based XSS: 클라이언트 측
- 대응: 입력값 검증, 출력 인코딩

#### CSRF (Cross-Site Request Forgery)
- CSRF Token 검증
- SameSite Cookie 속성
- Referer 검증

#### HTTP 취약점
- HTTP Response Splitting: CR(%0D)/LF(%0A) 인젝션
- HTTP Request Smuggling: 헤더 조작
- 부채널 공격

#### 추가 웹 취약점
- **Directory Traversal**: 경로 조작 (../../../etc/passwd)
- **파일 다운로드 취약점**: 임의 파일 다운로드
- **XXE** (XML External Entity): XML 파서 공격
- **SSRF** (Server-Side Request Forgery): 서버가 위조된 요청 수행
- **Insecure Deserialization**: 역직렬화 취약점
- **Open Redirect**: 외부 사이트로 리다이렉트

### 4.2 파일 업로드 취약점
- Content-Type 변조 (프록시 활용)
- 확장자 우회 기법
- 웹쉘 업로드 방지
- LimitRequestBody: 최대 파일 크기 제한

### 4.3 웹 서버 보안 설정

#### Apache 설정
- Directory Indexing: Options -Indexes
- .htaccess 설정
- httpd.conf 설정

#### 검색엔진 제어
- robots.txt: User-agent, Disallow 지시자

#### 쿠키 보안
- HttpOnly: JavaScript 접근 차단
- Secure: HTTPS만 전송
- SameSite: CSRF 방지

#### 세션 vs 쿠키
- **세션**: 서버 측 저장, 보안성 높음
- **쿠키**: 클라이언트 측 저장, 4KB 제한

#### 안전한 코딩 원칙
- 입력값 검증 (Input Validation)
- 출력값 인코딩 (Output Encoding)
- 화이트리스트 방식 적용
- 에러 메시지 최소화
- 최소권한 원칙

### 4.4 HTTP 메소드

#### GET vs POST 차이점
- GET: URL 파라미터, 길이 제한
- POST: Body 전송, 대용량 가능

#### 기타 메소드
- OPTIONS 메소드: 허용 메소드 확인
- PUT/DELETE 제한

## Part 5. 암호화 및 인증

### 5.1 암호화 기술

#### 대칭키 암호
- AES, DES, 3DES
- 블록암호 vs 스트림암호

#### 비대칭키 암호
- RSA, ECC
- 공개키/개인키 구조

#### 해시 함수
- **SHA 계열**: SHA-1 (160bit), SHA-256 (256bit), SHA-512 (512bit)
- **MD5**: 128bit (취약, 사용 비권장)
- **HMAC**: 키 기반 해시
- 무결성 검증
- Rainbow Table 공격
- Salt/PBKDF2 대응

#### 블록암호 운용 모드
- **ECB** (Electronic CodeBook):
  - 블록별 독립 암호화
  - 패턴 노출 취약점 (가장 단순)
- **CBC** (Cipher Block Chaining):
  - IV(초기화 벡터) 사용
  - 이전 블록과 XOR 후 암호화
- **CTR** (Counter):
  - 카운터 값 암호화
  - 병렬 처리 가능
- **GCM** (Galois/Counter Mode):
  - 인증 암호화 (AEAD)
  - 기밀성 + 무결성

#### 키 교환 및 서명
- **Diffie-Hellman**: 안전한 키 교환 프로토콜
- **디지털 서명**: 송신자 개인키로 암호화, 공개키로 검증
- **전자봉투**: 대칭키(데이터 암호화) + 비대칭키(키 암호화)

### 5.2 인증서 및 PKI

#### X.509 인증서
- CA (Certificate Authority)
- 인증서 체인

#### 기타
- PGP: CA 없는 신뢰 모델 (Web of Trust)
- 인증서 고정 (Certificate Pinning)
  - 고정 요소 3가지
  - SSL Interception 대응

#### 취약점
- 하트블리드 (HeartBleed): OpenSSL CVE-2014-0160

### 5.3 이메일 보안
- SPF (Sender Policy Framework): DNS 기반 발신자 검증
- DKIM: 도메인 키 인증
- DMARC: SPF+DKIM 통합
- S/MIME vs PGP 비교

## Part 6. 악성코드 및 포렌식

### 6.1 악성코드 분류

#### 바이러스 유형
- 조크 바이러스: 심리적 동요
- 파일/부트섹터 바이러스
- 매크로 바이러스

#### 기타 악성코드
- 웜: 자가 복제 및 전파
- 트로이목마: 위장 프로그램
- 랜섬웨어: 암호화 및 금전 요구
- 크립토재킹: 암호화폐 채굴
- APT: 지속적 표적 공격
- Zero-day exploit: 제로데이 공격

### 6.2 악성코드 분석

#### 분석 도구
- YARA: 패턴 기반 악성코드 분류
- 바이러스토탈 (VirusTotal)

#### 분석 방법
- 정적 분석: 코드 분석
- 동적 분석: 실행 분석
- 샌드박스 분석

### 6.3 포렌식
- 휘발성 순서: 레지스터→캐시→메모리→디스크
- 포렌식 유형
  - 디스크 포렌식
  - 메모리 포렌식
  - 네트워크 포렌식

### 6.4 사회공학 기법
- 피싱 및 변종 (스피어피싱, 웨일링)
- 스미싱, 큐싱
- 파밍

## Part 7. 보안 시스템

### 7.1 침입탐지시스템

#### IDS/IPS
- 시그니처 기반 vs 이상행위 기반
- 네트워크 기반 vs 호스트 기반

#### Snort/Suricata
- Snort 규칙: content, offset, depth
- Suricata: Snort 장점 + 대용량 트래픽 처리

### 7.2 보안관제
- SIEM: Security Information Event Management
- SOC 구성요소
  - 로그 수집
  - 이상 탐지
  - 실시간 대응

### 7.3 인증시스템
- SSO (Single Sign-On)
- 2FA/MFA (다중인증)
- 생체인증

### 7.4 취약점 스캐너
- NESSUS: 자동화된 취약점 스캔
- Tripwire: 파일 무결성 점검
- hping3: 패킷 생성 도구

### 7.5 보안 솔루션

#### 네트워크 보안 솔루션
- **WAF** (Web Application Firewall): 웹 애플리케이션 방화벽
- **UTM** (Unified Threat Management): 통합 위협 관리
- **NAC** (Network Access Control): 네트워크 접근 제어
- **DLP** (Data Loss Prevention): 데이터 유출 방지

#### 엔드포인트 보안
- **EDR** (Endpoint Detection and Response): 엔드포인트 위협 탐지 및 대응
- **백신/Anti-Malware**: 악성코드 탐지 및 제거
- **HIPS** (Host-based IPS): 호스트 기반 침입 방지

#### 로그 관리
- **syslog**: 표준 로그 프로토콜 (UDP 514)
- **rsyslog**: 확장 syslog (TCP 지원)
- **로그 레벨**: emergency(0), alert(1), critical(2), error(3), warning(4), notice(5), info(6), debug(7)

## Part 8. 백업 및 재해복구

### 8.1 백업 전략

#### 백업 유형
- **전체 백업** (Full Backup):
  - 모든 데이터 백업
  - 복구 시간 짧음, 저장공간 많이 필요
- **증분 백업** (Incremental Backup):
  - 마지막 백업 이후 변경된 데이터만
  - 저장공간 적음, 복구 시간 김
- **차등 백업** (Differential Backup):
  - 마지막 전체 백업 이후 변경된 데이터
  - 증분과 전체의 중간

#### 백업 주기 전략
- **3-2-1 규칙**:
  - 3개 복사본
  - 2개 다른 매체
  - 1개 오프사이트

### 8.2 재해복구 계획

#### 핵심 지표
- **RTO** (Recovery Time Objective): 목표 복구 시간
  - 시스템 중단 후 복구까지 허용 시간
- **RPO** (Recovery Point Objective): 목표 복구 시점
  - 데이터 손실 허용 범위

#### 업무연속성
- **BCP** (Business Continuity Plan): 업무연속성 계획
- **DRP** (Disaster Recovery Plan): 재해복구 계획
- **Hot Site**: 즉시 전환 가능 (비용 高)
- **Warm Site**: 수 시간~일 내 전환
- **Cold Site**: 장비만 준비 (비용 低)

## Part 9. 데이터베이스 보안

### 9.1 DB 접근통제
- 접근통제: 인증된 사용자 범위 내 접근
- 추론통제: 통계함수 관계 키값 유도 방지
- 흐름통제: 보안등급 간 정보흐름 제어

### 9.2 DB 보안 위협
- 집성 (Aggregation): 낮은 등급 정보 조합→높은 등급 정보
- 추론 (Inference): 정당한 정보로 높은 등급 접근
- 데이터 디들링: 원본 위변조

### 9.3 DB 보안 대책
- 동적 SQL 쿼리 금지
- 최소권한 원칙
- 입력값 검증

## Part 10. 개인정보보호 및 법령

### 10.1 개인정보보호법

#### 개인정보 정의
- 개인정보 정의: 식별가능한 정보

#### 개인정보처리자 의무
- 수집/이용 동의
- 최소수집 원칙
- 안전성 확보조치

#### 안전성 확보조치 기준
- 권한 부여/변경/말소 기록: 3년 보관
- 접속기록 점검: 월 1회 이상
- 접속기록 보존: 1년 이상 (5만명 이상 또는 고유식별정보/민감정보는 2년)

#### 개인정보 제3자 제공 고지사항
- 제공받는 자
- 이용 목적
- 제공 항목
- 보유/이용 기간
- 동의거부 권리 및 불이익

### 10.2 정보통신망법
- 정보통신망 정의: 전기통신설비 + 컴퓨터 및 이용기술
- 정보통신서비스 제공자 의무
  - 개인정보 암호화
  - 접근제어
  - 로그 보관
- 정보통신기반시설: 국가안전보장·행정·국방·치안·금융·통신·운송·에너지 관련 시스템

### 10.3 기타 법령 및 지침
- 전자금융감독규정: 금융 시스템 보안
- 국가정보보안기본지침: 공공기관 보안
- 전자서명법: 전자서명 인증

#### 주요 용어
- 내부관리계획: 개인정보 안전처리 계획
- 전자적 침해행위: 정상 인증 우회 공격
- 정보보호사전점검: 서비스 제공 전 보안점검
- 정보보호시스템: 정보 보호 하드웨어/소프트웨어

### 10.4 CCTV 운영

#### 개인정보보호법 제25조
- 안내판 설치: 설치목적/장소, 촬영범위/시간, 관리책임자 연락처
- 제한구역: 출입기록 병행, 촬영범위 최소화

## Part 11. 클라우드 및 최신 보안

### 11.1 클라우드 보안

#### 서비스 모델별 책임범위
- IaaS: OS, DB, 미들웨어, 응용프로그램 직접 관리
- PaaS: 응용프로그램만 관리
- SaaS: 계정/권한만 관리

#### 기타
- 공유책임모델
- 컨테이너/서버리스 보안

### 11.2 모바일 보안
- 딥링크 (Deeplink): 앱 직접 실행
- 루팅/탈옥 탐지
- 앱 난독화

### 11.3 IoT 보안
- 펌웨어 보안
- 디폴트 패스워드 변경
- 보안 업데이트

### 11.4 AI 보안
- 적대적 공격
- 모델 추출 공격
- 데이터 포이즈닝

## Part 12. 보안 인증 및 표준

### 12.1 취약점 관리

#### CVE (Common Vulnerabilities and Exposures)
- 명명규칙: CVE-연도-일련번호
- 예: CVE-2014-6628

#### CVSS (Common Vulnerability Scoring System)
- 취약점 점수화 시스템
- 공격난이도, 피해규모 평가

#### NVD
- National Vulnerability Database

### 12.2 CC 인증
- ISO/IEC 15408 국제표준
- EAL (Evaluation Assurance Level) 등급
- 보호프로파일

### 12.3 보안 표준
- ISO 27001/27002: 정보보호 관리체계
- NIST Cybersecurity Framework
- CIS Controls

### 12.4 산업별 보안 기준
- 금융: 전자금융감독규정
- 의료: HIPAA
- 카드: PCI-DSS

---

## 📌 실기 시험 핵심 체크리스트

### 1. 경로/파일 위치 암기
- Linux: `/etc/shadow`, `/var/log/*`, `/etc/xinetd.d/`
- Windows: `C:\Windows\System32\Logfiles\`

### 2. 명령어 정확히 암기
**Linux**
- `lastb`, `lastcomm`, `lsof`, `who`, `last`, `lastlog`
- `ps -ef`, `top`, `kill`, `killall`
- `netstat -ano`, `tcpdump`, `iptables`
- `chmod`, `chown`, `chgrp`, `umask`
- `find`, `grep`, `awk`, `sed`

**Windows**
- `net user`, `net localgroup`
- `netstat -ano`, `tasklist`, `taskkill`

**nmap 스캔**
- `-sT`, `-sS`, `-sU`, `-sN`, `-sF`, `-sX`, `-O`

### 3. 포트번호 (매우 중요!)
- FTP: 20(data), 21(control)
- SSH: 22
- Telnet: 23
- SMTP: 25
- DNS: 53
- HTTP: 80
- POP3: 110
- IMAP: 143
- SNMP: 161, 162
- LDAP: 389
- HTTPS: 443
- SSDP: 1900
- syslog: 514

### 4. 법령 조항 숙지
- 개인정보보호법 제17조, 제25조
- 안전성 확보조치 기준

### 5. 네트워크 기초 개념
- OSI 7계층, TCP/IP 4계층
- TCP 3-way/4-way handshake
- 서브넷팅 (CIDR 표기법)

### 6. 접근통제 모델
- DAC, MAC, RBAC
- Bell-LaPadula, Biba 모델

### 7. 암호화 기초
- 대칭키 vs 비대칭키
- 블록암호 운용 모드 (ECB, CBC, CTR, GCM)
- 해시 함수 및 디지털 서명

### 8. 메모리 보호 기법
- ASLR, DEP/NX, Stack Canary
- Buffer Overflow, ROP

### 9. 공격기법 원리 이해
- 각 공격의 동작원리와 대응방법
- DoS/DDoS 공격 유형
- 웹 취약점 (OWASP Top 10)

### 10. 백업 및 재해복구
- 백업 유형 (Full, Incremental, Differential)
- RTO, RPO 개념

### 11. 약어 풀네임 암기
**보안 모델/체계**
- CIA: Confidentiality, Integrity, Availability
- AAA: Authentication, Authorization, Accounting
- ISMS-P: Information Security Management System - Personal information
- CVSS: Common Vulnerability Scoring System
- CVE: Common Vulnerabilities and Exposures

**기술/솔루션**
- ASLR: Address Space Layout Randomization
- DEP: Data Execution Prevention
- WAF: Web Application Firewall
- IDS/IPS: Intrusion Detection/Prevention System
- SIEM: Security Information Event Management
- EDR: Endpoint Detection and Response
- DLP: Data Loss Prevention
- UTM: Unified Threat Management
- NAC: Network Access Control

**공격 기법**
- XSS: Cross-Site Scripting
- CSRF: Cross-Site Request Forgery
- SSRF: Server-Side Request Forgery
- XXE: XML External Entity
- ROP: Return Oriented Programming

**네트워크**
- ARP: Address Resolution Protocol
- DHCP: Dynamic Host Configuration Protocol
- NAT/PAT: Network Address Translation / Port Address Translation
- VPN: Virtual Private Network
- VLAN: Virtual Local Area Network

**재해복구**
- BCP: Business Continuity Plan
- DRP: Disaster Recovery Plan
- RTO: Recovery Time Objective
- RPO: Recovery Point Objective
