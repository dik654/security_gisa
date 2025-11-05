# 정보보안기사 실기 완전 정리 (PDCA 사이클 기반)

> **학습 전략**: 보안은 계획(Plan) → 실행(Do) → 점검(Check) → 개선(Act)의 순환 구조입니다.
> 이 흐름을 따라가며 학습하면 체계적으로 개념을 이해할 수 있습니다.

---

# 📋 PDCA 사이클 개요

## Plan (계획) - 보안 체계 수립
- 정보보호 관리체계 (ISMS-P)
- 위험관리 및 평가
- 접근통제 모델 및 보안 원칙
- 보안 정책 수립

## Do (실행) - 보안 통제 구현
- 시스템 보안 (Linux/Windows)
- 네트워크 보안 (프로토콜, 장비)
- 암호화 구현
- 웹 애플리케이션 보안
- 데이터베이스 보안

## Check (점검) - 보안 모니터링
- 침입탐지/방지 시스템 (IDS/IPS)
- 보안관제 (SOC/SIEM)
- 취약점 스캐닝
- 로그 분석

## Act (개선) - 대응 및 개선
- 침해사고 대응
- 악성코드 분석 및 제거
- 포렌식 조사
- 법령 준수 및 개선
- 재해복구 및 백업

---

# 📌 PLAN (계획) - 보안 체계 수립

## Part 1. 보안 기본 원칙

### 1.1 보안 3요소 (CIA Triad)
- **기밀성 (Confidentiality)**: 인가된 사용자만 접근
  - 암호화, 접근통제로 구현
- **무결성 (Integrity)**: 인가된 방법으로만 변경
  - 해시, 디지털 서명으로 구현
- **가용성 (Availability)**: 필요시 접근 가능
  - 이중화, 백업으로 구현

### 1.2 AAA (Triple A)
- **인증 (Authentication)**: 신원 확인
  - 알고 있는 것 (비밀번호)
  - 가지고 있는 것 (OTP, 토큰)
  - 신체적 특징 (지문, 홍채)
- **인가 (Authorization)**: 권한 부여
  - 접근 권한 할당
- **계정관리 (Accounting)**: 사용 추적
  - 로그 기록 및 감사

### 1.3 기타 보안 원칙
- **부인방지 (Non-repudiation)**: 디지털 서명
- **최소권한 원칙 (Principle of Least Privilege)**
- **직무분리 (Separation of Duties)**
- **다층방어 (Defense in Depth)**
- **Fail-Safe**: 장애시 안전한 상태로
- **Fail-Secure**: 장애시 보안 우선
- **Need-to-Know**: 필요한 정보만 제공
- **알아야 할 필요성 (Need to Know)**

---

## Part 2. 접근통제 모델

### 2.1 접근통제 유형
- **DAC (Discretionary Access Control)**: 임의적 접근통제
  - 소유자가 권한 결정
  - 유연하지만 보안 수준 낮음
  - 예: Unix/Linux 파일 권한 (rwx)

- **MAC (Mandatory Access Control)**: 강제적 접근통제
  - 시스템이 보안등급 기반으로 결정
  - 보안 수준 높지만 유연성 낮음
  - 예: SELinux, 군사 시스템

- **RBAC (Role-Based Access Control)**: 역할 기반 접근통제
  - 역할(Role)에 권한 부여
  - 관리 용이
  - 예: 회사 조직 구조

### 2.2 보안 모델

#### Bell-LaPadula 모델 (기밀성 중심)
- **No Read Up (상향 읽기 금지)**: 상위 등급 읽기 불가
- **No Write Down (하향 쓰기 금지)**: 하위 등급 쓰기 불가
- **용도**: 군사, 정부 기밀 문서

#### Biba 모델 (무결성 중심)
- **No Write Up (상향 쓰기 금지)**: 상위 등급 쓰기 불가
- **No Read Down (하향 읽기 금지)**: 하위 등급 읽기 불가
- **용도**: 데이터 무결성 중요한 시스템

#### Clark-Wilson 모델
- 무결성 중심, 상업용 환경
- 잘 정의된 트랜잭션 (Well-Formed Transaction)
- 직무 분리 (Separation of Duties)

#### Chinese Wall 모델
- 이해충돌 방지 (Conflict of Interest)
- 동일 이해집단 내 정보 접근 제한
- 예: 금융기관 컨설팅

---

## Part 3. 정보보호 관리체계 (ISMS-P)

### 3.1 ISMS-P 인증체계

#### 3개 영역 구조
1. **관리체계 수립 및 운영** (16개 항목)
   - 경영진 책임 및 조직 구성
   - 위험관리
   - 정보보호 대책
   - 사후관리

2. **보호대책 요구사항** (64개 항목)
   - 정책/조직/자산 관리
   - 인적 보안
   - 외부자 보안
   - 물리적 보안
   - 인증 및 권한 관리
   - 접근통제
   - 암호화
   - 정보시스템 도입 및 개발 보안
   - 시스템 및 서비스 운영관리
   - 시스템 및 서비스 보안관리
   - 사고 예방 및 대응

3. **개인정보 처리 단계별 요구사항** (22개 항목)
   - 개인정보 수집/이용/제공
   - 개인정보 파기
   - 정보주체 권리 보호

#### ISMS-P 인증 절차
1. 인증신청
2. 심사계획 수립
3. 문서심사
4. 현장심사
5. 인증위원회 심의
6. 인증서 발급
7. 사후관리 (연 1회 이상)

### 3.2 정보보호 조직
- **CISO (Chief Information Security Officer)**: 정보보호 최고책임자
- **CPO (Chief Privacy Officer)**: 개인정보보호 책임자
- **SOC (Security Operation Center)**: 보안관제센터
- **CERT (Computer Emergency Response Team)**: 침해사고 대응팀
- **CSIRT (Computer Security Incident Response Team)**: 보안사고 대응팀

### 3.3 물리적 보안
- **출입통제 시스템**
  - 생체인증, 카드인증
  - 이중 인증
- **CCTV 설치 및 운영** (개인정보보호법 제25조)
  - 안내판 설치: 설치목적/장소, 촬영범위/시간, 관리책임자 연락처
  - 제한구역: 출입기록 병행, 촬영범위 최소화
- **보안구역 설정**
  - 통제구역, 제한구역, 개방구역
- **Clear Desk/Clear Screen 정책**
  - 업무 종료 시 책상 정리
  - 화면 잠금

### 3.4 인적 보안
- **보안 서약서** 징구
- **보안 교육 및 훈련** (연 1회 이상)
- **직무 분리 (Separation of Duties)**
- **업무 인수인계 절차**
- **퇴직자 보안 조치**
  - 계정 삭제
  - 자산 반납
  - 보안 서약 연장

---

## Part 4. 위험관리

### 4.1 위험평가 방법론

#### 베이스라인 접근법 (Baseline Approach)
- 체크리스트 기반 표준화된 보안대책
- 빠르고 간편
- 조직 특성 반영 어려움

#### 비정형 접근법 (Informal Approach)
- 경험자의 지식과 판단 활용
- 유연하지만 일관성 부족

#### 상세 위험분석 (Detailed Risk Analysis)
- 자산 식별 → 위협 식별 → 취약점 분석 → 위험도 산정
- 정확하지만 시간/비용 많이 소요

#### 복합 접근법 (Combined Approach)
- 위 세 가지 방법 혼합
- 가장 효과적

### 4.2 위험분석 구성요소
- **자산 (Asset)**: 보호해야 할 대상
  - 정보자산, 물리적 자산, 인적 자산
  - **자산 가치 평가**: CIA 기준
- **위협 (Threat)**: 자산에 손실을 입힐 수 있는 요인
  - 자연적 위협 (재해, 화재)
  - 인적 위협 (내부자, 해커)
  - 환경적 위협 (정전, 온도)
- **취약점 (Vulnerability)**: 위협에 노출될 수 있는 약점
- **위험 (Risk)**: 위협이 취약점을 이용하여 자산에 손실을 입힐 가능성

#### 위험도 계산
```
위험도 = 자산가치 × 위협 × 취약점
```

### 4.3 위험대응 전략

#### 위험 수용 (Risk Accept)
- 일정 수준 이하 위험 수용
- 비용 효율적
- 잔여 위험 (Residual Risk)

#### 위험 회피 (Risk Avoid)
- 위험한 프로세스/활동 포기
- 가장 확실하지만 업무 제한

#### 위험 전가 (Risk Transfer)
- 보험 가입
- 아웃소싱, 외주
- 제3자에게 위험 이전

#### 위험 감소/완화 (Risk Mitigate)
- 보안대책 구현으로 위험 낮춤
- 가장 일반적인 방법
- 예: 방화벽, 백신, 암호화

---

## Part 5. 보안 정책 및 표준

### 5.1 보안 정책 체계
- **정책 (Policy)**: 최상위 문서, 경영진 승인
- **표준 (Standard)**: 정책 구현 기준
- **지침 (Guideline)**: 권고사항
- **절차 (Procedure)**: 상세 실행 방법

### 5.2 국가정보보안기본지침
- **적용 대상**: 국가 공공기관
- **주요 내용**:
  - 국가사이버안전관리 체계
  - 정보통신기반시설 보호
  - 암호장비 관리
  - 보안감사

### 5.3 주요 보안 표준

#### ISO/IEC 27001
- 정보보호 관리체계 국제 표준
- PDCA 사이클 기반

#### ISO/IEC 27002
- 정보보호 통제 지침
- 114개 통제 항목

#### NIST Cybersecurity Framework
- 미국 NIST 사이버보안 프레임워크
- 5대 기능: Identify, Protect, Detect, Respond, Recover

#### CIS Controls
- 20개 주요 보안 통제

#### CC 인증 (Common Criteria)
- ISO/IEC 15408 국제표준
- **EAL (Evaluation Assurance Level)** 등급
  - EAL1 (기능 테스트) ~ EAL7 (형식 검증)
- **보호프로파일 (Protection Profile)**

---

# 🔧 DO (실행) - 보안 통제 구현

## Part 6. 암호화

### 6.1 대칭키 암호 알고리즘

#### DES (Data Encryption Standard)
- **키 길이**: 56비트 (실제 64비트, 8비트는 패리티)
- **블록 크기**: 64비트
- **라운드 수**: 16라운드
- **구조**: Feistel 구조
- **상태**: 취약 (사용 비권장)

#### 3DES (Triple DES)
- **키 길이**: 168비트 (56비트 × 3)
- **방식**: DES를 3번 적용 (암호화-복호화-암호화)
- **상태**: 느림, AES로 대체 권장

#### AES (Advanced Encryption Standard)
- **키 길이**: 128비트, 192비트, 256비트
- **블록 크기**: 128비트
- **라운드 수**: 10라운드(128비트), 12라운드(192비트), 14라운드(256비트)
- **구조**: SPN (Substitution-Permutation Network)
- **현재 표준**: 가장 널리 사용

#### 국내 암호 알고리즘 (KISA 권장)

**SEED**
- **키 길이**: 128비트
- **블록 크기**: 128비트
- **라운드 수**: 16라운드
- **구조**: Feistel 구조
- **용도**: 전자상거래, 금융

**ARIA**
- **키 길이**: 128비트, 192비트, 256비트
- **블록 크기**: 128비트
- **라운드 수**: 12/14/16라운드
- **용도**: AES 대체 국산 암호

**LEA (Lightweight Encryption Algorithm)**
- **키 길이**: 128비트, 192비트, 256비트
- **블록 크기**: 128비트
- **특징**: 경량 암호, 소프트웨어 최적화

**HIGHT (HIGh security and lightweight)**
- **키 길이**: 128비트
- **블록 크기**: 64비트
- **특징**: IoT, RFID용 경량 암호

#### 스트림 암호

**RC4**
- 가변 키 길이 (40-2048비트)
- **상태**: 취약 (WEP, WPA 공격), 사용 비권장

**ChaCha20**
- 256비트 키
- RC4 대체, TLS 1.3에서 사용

### 6.2 블록암호 운용 모드

#### ECB (Electronic CodeBook)
- **특징**: 블록별 독립 암호화
- **장점**: 병렬 처리 가능, 빠름
- **단점**: 동일 평문 → 동일 암호문 (패턴 노출)
- **사용**: 비권장

#### CBC (Cipher Block Chaining)
- **특징**: IV(초기화 벡터) 사용, 이전 블록과 XOR
- **장점**: 패턴 숨김
- **단점**: 순차 처리, 에러 전파
- **사용**: 파일 암호화

#### CTR (Counter Mode)
- **특징**: 카운터 값을 암호화
- **장점**: 병렬 처리 가능, 에러 전파 없음
- **사용**: 디스크 암호화

#### GCM (Galois/Counter Mode)
- **특징**: CTR + 인증 (AEAD - Authenticated Encryption with Associated Data)
- **제공**: 기밀성 + 무결성
- **사용**: TLS 1.3, IPSec

### 6.3 비대칭키 암호 알고리즘

#### RSA (Rivest-Shamir-Adleman)
- **키 길이**: 1024비트, 2048비트, 4096비트 (2048비트 이상 권장)
- **기반**: 소인수분해 난제
- **용도**: 
  - 디지털 서명
  - 키 교환
  - 전자봉투
- **속도**: 느림 (대칭키 암호와 결합 사용)

#### ECC (Elliptic Curve Cryptography)
- **키 길이**: 256비트 (RSA 3072비트와 동등)
- **장점**: 짧은 키로 높은 보안성
- **용도**: 모바일, IoT

#### Diffie-Hellman
- **용도**: 키 교환 프로토콜
- **방식**: 공개 채널에서 안전하게 키 교환
- **DHE**: Ephemeral DH (일회용 키)

### 6.4 해시 함수

#### MD5 (Message Digest 5)
- **출력**: 128비트
- **상태**: 충돌 취약점 발견, 사용 비권장

#### SHA-1 (Secure Hash Algorithm 1)
- **출력**: 160비트
- **상태**: 충돌 취약점 발견, 사용 비권장

#### SHA-2 계열
- **SHA-256**: 256비트 (현재 표준)
- **SHA-384**: 384비트
- **SHA-512**: 512비트
- **용도**: 무결성 검증, 디지털 서명

#### SHA-3
- **출력**: 224/256/384/512비트
- **구조**: Keccak (SHA-2와 다른 구조)
- **상태**: 최신 표준

#### HMAC (Hash-based Message Authentication Code)
- **특징**: 키 기반 해시
- **용도**: 메시지 인증, 무결성
- **예**: HMAC-SHA256

### 6.5 패스워드 해싱

#### 일반 해시의 문제점
- Rainbow Table 공격
- Brute Force 공격

#### Salt
- 패스워드에 랜덤 값 추가
- Rainbow Table 무력화

#### PBKDF2 (Password-Based Key Derivation Function 2)
- 반복 해싱 (수천~수만 회)
- 키 유도 함수
- **KISA 권장**: 10,000회 이상

#### bcrypt
- Blowfish 기반
- Cost Factor (반복 횟수)
- **권장**: 현재 널리 사용

#### scrypt
- 메모리 집약적
- GPU 공격 방어

#### Argon2
- **최신 표준** (Password Hashing Competition 우승)
- 메모리 하드 함수
- **권장**: 2015년 이후 최고

### 6.6 디지털 서명 및 PKI

#### 디지털 서명 프로세스
1. 송신자: 메시지 해시 → 개인키로 암호화 → 서명
2. 수신자: 공개키로 복호화 → 해시 비교 → 검증

#### 제공 기능
- 인증 (Authentication)
- 무결성 (Integrity)
- 부인방지 (Non-repudiation)

#### 전자봉투 (Digital Envelope)
1. 대칭키로 메시지 암호화 (빠름)
2. 수신자 공개키로 대칭키 암호화 (안전)
3. 암호화된 메시지 + 암호화된 키 전송

#### X.509 인증서
- **발급자**: CA (Certificate Authority)
- **구성**:
  - 버전
  - 일련번호
  - 서명 알고리즘
  - 발급자 (Issuer)
  - 유효기간
  - 주체 (Subject)
  - 공개키
  - CA 서명

#### 인증서 체인
- Root CA → Intermediate CA → End Entity
- 신뢰 체인 검증

#### 인증서 폐기
- **CRL (Certificate Revocation List)**: 폐기 목록
- **OCSP (Online Certificate Status Protocol)**: 실시간 검증

#### PGP (Pretty Good Privacy)
- Web of Trust 모델
- CA 없이 사용자 간 신뢰
- 이메일 암호화

#### 인증서 고정 (Certificate Pinning)
- 특정 인증서/공개키만 신뢰
- **고정 요소**: 인증서, 공개키, 해시
- **목적**: Man-in-the-Middle 공격 방지

---

### 7.7 네트워크 공격 기법

#### DoS/DDoS 공격

**SYN Flooding**
- TCP 3-way handshake 악용
- SYN만 보내고 ACK 안 보냄
- 서버 연결 대기 큐 고갈
- **대응**: SYN Cookie, 방화벽 필터링

**Smurf Attack**
- ICMP Echo Request를 브로드캐스트 주소로 전송
- 출발지 IP를 피해자로 위조
- 증폭 공격 (Amplification)
- **대응**: Directed Broadcast 차단

**Land Attack**
- 출발지 IP = 목적지 IP로 설정
- 시스템 혼란
- **대응**: 출발지/목적지 동일 패킷 차단

**Teardrop**
- Fragment Offset 값 조작
- 조각 패킷 중첩
- **대응**: 패킷 필터링

**Ping of Death**
- ICMP 패킷 크기를 65,535바이트 초과
- 버퍼 오버플로우
- **대응**: 패킷 크기 제한

**SSDP DRDoS**
- SSDP 프로토콜 (UDP 1900) 악용
- IoT 장비 대상 반사 증폭 공격
- **대응**: SSDP 포트 차단

**Slowloris (Slow HTTP Header DoS)**
- HTTP 헤더를 천천히 전송
- Connection 유지로 자원 고갈
- CRLF(\r\n\r\n) 전송 안 함
- **대응**: Connection Timeout 설정

**HTTP GET Flooding**
- 정상 GET 요청을 대량 전송
- 애플리케이션 계층 DDoS
- **대응**: Rate Limiting, WAF

#### 스캔 공격

**nmap 스캔 기법 (🔴 기출 빈출)**

**-sT (TCP Connect Scan)**
- 전체 3-way handshake 수행
- 로그 기록됨
- 일반 사용자 권한 가능

**-sS (SYN Scan, Stealth Scan)**
- SYN만 보내고 RST로 종료
- 반개방 스캔 (Half-Open)
- 로그 회피
- root 권한 필요

**-sU (UDP Scan)**
- UDP 포트 스캔
- 느림

**-sN (Null Scan)**
- 플래그 없음
- 필터링 우회

**-sF (FIN Scan)**
- FIN 플래그만 설정
- 필터링 우회

**-sX (XMAS Scan)**
- FIN+PSH+URG 플래그 설정
- 크리스마스 트리처럼 모든 플래그 ON
- 필터링 우회

**-sA (ACK Scan)**
- 방화벽 규칙 탐지
- 포트 개방 여부는 알 수 없음

**-O (OS Detection)**
- TCP/IP 스택 핑거프린팅으로 OS 탐지

**-D (Decoy Scan)**
- 위장 IP 사용
- 출발지 숨김

**-sV (Version Detection)**
- 서비스 버전 탐지

**-A (Aggressive Scan)**
- OS 탐지 + 버전 탐지 + 스크립트 + traceroute

**-p (Port Specification)**
- 특정 포트 지정
- 예: `-p 80,443` 또는 `-p 1-1000`

**-Pn (No Ping)**
- 호스트 발견 단계 생략
- 방화벽이 ICMP 차단시 유용

#### 스푸핑/하이재킹

**ARP Spoofing/Poisoning**
- ARP 캐시에 거짓 정보 주입
- 중간자 공격 (MITM)
- **대응**: Static ARP, DAI (Dynamic ARP Inspection)

**IP Spoofing**
- 출발지 IP 위조
- DDoS 공격에 활용
- **대응**: Ingress Filtering

**DNS Spoofing/Cache Poisoning**
- DNS 캐시에 거짓 정보 삽입
- 사용자를 가짜 사이트로 유도
- **대응**: DNSSEC, 캐시 무작위화

**세션 하이재킹 (Session Hijacking)**
- 연결된 세션 가로채기
- **유형**:
  - TCP 세션 하이재킹: Sequence Number 예측
  - 웹 세션 하이재킹: 쿠키 탈취
- **대응**: 암호화, 세션 토큰 재발급

**Switch Jamming (MAC Flooding)**
- MAC 테이블 오버플로우
- 스위치를 허브처럼 동작시킴
- **대응**: Port Security

#### 기타 공격

**HTTP Request Smuggling**
- Content-Length와 Transfer-Encoding 헤더 조작
- 프록시와 서버 간 해석 차이 악용
- **대응**: 헤더 정규화, 엄격한 파싱

**Credential Stuffing**
- 다른 사이트에서 탈취한 계정 정보 재사용
- 대량 자동 로그인 시도
- **대응**: MFA, CAPTCHA, Rate Limiting

**Directed Broadcast**
- 브로드캐스트 주소로 패킷 전송
- Smurf 공격에 악용
- **대응**: `no ip directed-broadcast` (라우터 설정)

---

### 7.8 네트워크 보안 프로토콜

#### IPSec (IP Security) 🔴 기출 빈출

**구성 요소**

**AH (Authentication Header)**
- **제공**: 무결성, 인증
- **미제공**: 기밀성 (암호화 X)
- **보호 범위**: IP 헤더 + 페이로드
- **프로토콜 번호**: 51

**ESP (Encapsulating Security Payload)**
- **제공**: 기밀성, 무결성, 인증
- **암호화**: 페이로드 암호화
- **프로토콜 번호**: 50
- **권장**: AH보다 ESP 사용

**동작 모드**

**전송 모드 (Transport Mode)**
- **보호 대상**: 페이로드만 보호
- **IP 헤더**: 원본 유지
- **용도**: 호스트 간 통신 (End-to-End)
- **사용 예**: 클라이언트 ↔ 서버 직접 통신

**터널 모드 (Tunnel Mode)**
- **보호 대상**: 전체 IP 패킷 보호
- **IP 헤더**: 새로운 IP 헤더 추가
- **용도**: 게이트웨이 간 통신 (VPN)
- **사용 예**: 본사 ↔ 지사 VPN

**IKE (Internet Key Exchange)**
- IPSec 키 교환 프로토콜
- UDP 500 포트
- **Phase 1**: ISAKMP SA 수립
- **Phase 2**: IPSec SA 수립

**SA (Security Association)**
- 보안 파라미터 집합
- 단방향 (송신/수신 각각)
- SPI (Security Parameter Index)로 식별

#### TLS/SSL

**TLS 1.2**
- 현재 가장 널리 사용
- 암호 스위트: RSA, AES, SHA

**TLS 1.3**
- 최신 표준 (2018년)
- 0-RTT (빠른 연결)
- 암호 스위트 간소화
- 취약한 알고리즘 제거

**Handshake 과정**
1. Client Hello (지원 암호 스위트)
2. Server Hello (선택된 암호 스위트, 인증서)
3. Key Exchange (공개키/Diffie-Hellman)
4. Finished (암호화 통신 시작)

#### VPN 프로토콜

**PPTP (Point-to-Point Tunneling Protocol)**
- 포트: TCP 1723
- 취약: 암호화 약함 (사용 비권장)

**L2TP (Layer 2 Tunneling Protocol)**
- 포트: UDP 1701
- 암호화 미제공 → IPSec과 결합 (L2TP/IPSec)

**OpenVPN**
- SSL/TLS 기반
- 강력한 암호화
- 오픈소스

**WireGuard**
- 최신 VPN 프로토콜
- 빠르고 간결한 코드
- ChaCha20, Curve25519 사용

#### 인증 프로토콜

**RADIUS (Remote Authentication Dial-In User Service)**
- 포트: UDP 1812(인증), 1813(어카운팅)
- 중앙 집중식 AAA
- 무선 AP, VPN 인증에 사용

**TACACS+ (Terminal Access Controller Access-Control System Plus)**
- 포트: TCP 49
- Cisco 독점
- 전체 패킷 암호화 (RADIUS는 비밀번호만)

**Kerberos**
- 티켓 기반 인증
- KDC (Key Distribution Center)
- SSO 구현

**EAP (Extensible Authentication Protocol)**
- 인증 프레임워크
- **유형**:
  - EAP-TLS: 인증서 기반
  - EAP-TTLS: 터널링
  - EAP-PEAP: Protected EAP
  - EAP-MD5: 약함 (비권장)

#### 이메일 보안

**SPF (Sender Policy Framework)**
- DNS TXT 레코드
- 발신 서버 IP 검증
- 예: `v=spf1 ip4:1.2.3.4 -all`

**DKIM (DomainKeys Identified Mail)**
- 디지털 서명
- 도메인 키 검증
- 헤더 + 본문 서명

**DMARC (Domain-based Message Authentication, Reporting & Conformance)**
- SPF + DKIM 통합
- 정책 설정: none, quarantine, reject

**S/MIME vs PGP**
- **S/MIME**: PKI 기반, 인증서 필요, 기업 환경
- **PGP**: Web of Trust, 인증서 불필요, 개인 사용

---

## Part 8. 시스템 보안 - Linux/Unix

### 8.1 주요 시스템 파일 및 디렉토리 🔴 암기 필수

#### 계정 관련
- `/etc/passwd`: 사용자 계정 정보
  - 형식: `username:x:UID:GID:GECOS:home:shell`
- `/etc/shadow`: 암호화된 패스워드 (root만 읽기 가능)
  - 형식: `username:password:lastchange:min:max:warn:inactive:expire`
- `/etc/group`: 그룹 정보
- `/etc/gshadow`: 그룹 패스워드
- `/etc/login.defs`: 패스워드 정책
  - `PASS_MIN_LEN 8`: 최소 길이
  - `PASS_MAX_DAYS 90`: 최대 사용일
  - `PASS_MIN_DAYS 1`: 최소 사용일
  - `PASS_WARN_AGE 7`: 경고 일수

#### 네트워크 접근 제어
- `/etc/hosts.allow`: 접근 허용 호스트 (tcpwrapper)
- `/etc/hosts.deny`: 접근 거부 호스트
- `/etc/hosts`: 호스트 이름 - IP 매핑

#### 로그 파일 (🔴 기출 빈출)
- `/var/log/messages` 또는 `/var/log/syslog`: 시스템 로그
- `/var/log/secure`: 인증 관련 로그 (su, sudo, SSH, 원격 접속)
- `/var/log/auth.log`: 인증 로그 (Debian/Ubuntu)
- `/var/log/wtmp`: 로그인/로그아웃/재부팅 기록
- `/var/log/btmp`: 실패한 로그인 기록 (5회 이상)
- `/var/log/lastlog`: 마지막 로그인 정보
- `/var/log/utmp`: 현재 로그인 사용자
- `/var/log/acct` 또는 `/var/log/pacct`: 프로세스 계정 정보

#### PAM (Pluggable Authentication Modules)
- `/etc/pam.d/`: PAM 설정 파일 디렉토리
  - `/etc/pam.d/sshd`: SSH PAM 설정
  - `/etc/pam.d/login`: 로그인 PAM 설정
  - `/etc/pam.d/sudo`: sudo PAM 설정
- `/lib/security/` 또는 `/lib64/security/`: PAM 모듈 위치
- `/etc/security/`: PAM 모듈 추가 설정
  - `/etc/security/limits.conf`: 리소스 제한
  - `/etc/security/access.conf`: 접근 제어

#### SSH 설정
- `/etc/ssh/sshd_config`: SSH 서버 설정
  - `PermitRootLogin no`: root 로그인 금지
  - `PasswordAuthentication no`: 패스워드 인증 금지 (공개키만)
  - `Port 22`: 포트 변경 권장
  - `Protocol 2`: SSH v2만 사용
- `/etc/ssh/ssh_config`: SSH 클라이언트 설정

#### 기타 중요 파일
- `/etc/resolv.conf`: DNS 서버 설정
- `/etc/fstab`: 파일시스템 마운트 정보
- `/etc/crontab`: 시스템 cron 설정
- `/etc/cron.allow`, `/etc/cron.deny`: cron 접근 제어
- `/etc/sudoers`: sudo 권한 설정
- `/etc/xinetd.d/`: xinetd 서비스 설정
- `/etc/selinux/config`: SELinux 설정

### 8.2 파일 권한 관리 🔴 필수

#### 기본 권한
```
rwx rwx rwx
│││ │││ │││
│││ │││ └┴┴─ Others
│││ └┴┴───── Group
└┴┴─────── Owner

r = 4 (Read)
w = 2 (Write)
x = 1 (Execute)
```

**chmod 명령어**
```bash
chmod 755 file    # rwxr-xr-x
chmod 644 file    # rw-r--r--
chmod 600 file    # rw-------
```

#### 특수 권한

**SetUID (4000)**
- 파일 실행 시 소유자 권한으로 실행
- 예: `/usr/bin/passwd` (일반 사용자가 /etc/shadow 수정 가능)
- 표시: `rwsr-xr-x`
- 설정: `chmod 4755 file` 또는 `chmod u+s file`

**SetGID (2000)**
- 파일: 그룹 권한으로 실행
- 디렉토리: 생성 파일이 디렉토리 그룹 상속
- 표시: `rwxr-sr-x`
- 설정: `chmod 2755 file` 또는 `chmod g+s file`

**Sticky Bit (1000)**
- 디렉토리: 소유자만 파일 삭제 가능
- 예: `/tmp` 디렉토리
- 표시: `rwxrwxrwt`
- 설정: `chmod 1777 dir` 또는 `chmod +t dir`

#### umask
- 기본 권한 마스크
- 계산: `666 - umask` (파일), `777 - umask` (디렉토리)
- 예: umask 022
  - 파일: 666 - 022 = 644 (rw-r--r--)
  - 디렉토리: 777 - 022 = 755 (rwxr-xr-x)

#### chown, chgrp
```bash
chown user file           # 소유자 변경
chown user:group file     # 소유자 및 그룹 변경
chgrp group file          # 그룹 변경
chown -R user:group dir   # 재귀적 변경
```

### 8.3 주요 명령어 🔴 기출 빈출

#### 로그 관련
```bash
last            # wtmp 로그 (로그인 기록)
lastb           # btmp 로그 (실패한 로그인)
lastlog         # lastlog 로그 (마지막 로그인)
lastcomm        # pacct 로그 (프로세스 실행 기록)
who             # 현재 로그인 사용자 (utmp)
w               # 현재 로그인 사용자 + 실행 중인 작업
```

#### 프로세스 관리
```bash
ps -ef          # 모든 프로세스 목록
ps aux          # BSD 스타일 프로세스 목록
top             # 실시간 프로세스 모니터링
htop            # 향상된 top (설치 필요)
kill -9 PID     # 프로세스 강제 종료 (SIGKILL)
kill -15 PID    # 프로세스 정상 종료 (SIGTERM)
killall name    # 프로세스명으로 종료
pkill name      # 패턴 매칭으로 종료
```

#### 네트워크
```bash
netstat -ano    # 네트워크 연결 상태
netstat -tuln   # TCP/UDP 리스닝 포트
ss -tuln        # netstat 대체 (더 빠름)
lsof -i :80     # 80번 포트 사용 프로세스
tcpdump -i eth0 # 패킷 캡처
  -c 100        # 100개 패킷만
  -w file.pcap  # 파일 저장
  -r file.pcap  # 파일 읽기
  -n            # IP 주소 표시 (DNS 조회 X)
ifconfig        # 네트워크 인터페이스 (구형)
ip addr         # 네트워크 인터페이스 (신형)
ip route        # 라우팅 테이블
route -n        # 라우팅 테이블 (구형)
arp -a          # ARP 캐시
```

#### DNS 조회
```bash
nslookup domain.com     # DNS 조회 (구형)
dig domain.com          # DNS 조회 (신형, 상세)
dig @8.8.8.8 domain.com # 특정 DNS 서버 조회
host domain.com         # 간단한 DNS 조회
```

#### 파일 검색
```bash
find / -name "*.conf"         # 파일명으로 검색
find / -type f -perm 4000     # SetUID 파일 찾기
find / -mtime -7              # 7일 내 수정된 파일
grep "pattern" file           # 패턴 검색
grep -r "pattern" /etc        # 재귀 검색
grep -i "pattern" file        # 대소문자 무시
```

#### 방화벽 (iptables)
```bash
iptables -L                   # 규칙 목록
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    # 규칙 추가
iptables -D INPUT 1           # 규칙 삭제 (1번)
iptables -F                   # 모든 규칙 삭제
iptables -P INPUT DROP        # 기본 정책 설정

# 옵션
-t table    # nat, filter, mangle
-A chain    # 규칙 추가 (INPUT, OUTPUT, FORWARD)
-D chain    # 규칙 삭제
-L          # 목록
-F          # 초기화
-P policy   # 기본 정책 (ACCEPT, DROP)
-s source   # 출발지 IP
-d dest     # 목적지 IP
-p protocol # 프로토콜 (tcp, udp, icmp)
--dport     # 목적지 포트
--sport     # 출발지 포트
-j target   # 액션 (ACCEPT, DROP, REJECT)
```

#### 기타 유용한 명령어
```bash
nc (netcat)         # 네트워크 연결 및 포트 스캔
  nc -l -p 1234     # 1234 포트 리스닝
  nc host 1234      # host 1234 포트 접속
curl URL            # HTTP 요청
wget URL            # 파일 다운로드
```

---

## Part 9. 시스템 보안 - Windows

### 9.1 이벤트 로그 🔴 기출 필수

#### 주요 보안 이벤트 ID
- **4624**: 계정 로그온 성공
- **4625**: 계정 로그온 실패
- **4634**: 계정 로그오프
- **4648**: 명시적 자격 증명을 사용한 로그온 시도
- **4672**: 특수 권한이 할당된 로그온 (관리자)
- **4720**: 사용자 계정 생성
- **4722**: 사용자 계정 활성화
- **4723**: 패스워드 변경 시도
- **4724**: 패스워드 재설정 시도
- **4725**: 사용자 계정 비활성화
- **4726**: 사용자 계정 삭제
- **4728**: 보안 그룹에 사용자 추가
- **4729**: 보안 그룹에서 사용자 제거
- **4732**: 로컬 그룹에 사용자 추가
- **4733**: 로컬 그룹에서 사용자 제거
- **4740**: 사용자 계정 잠금
- **4767**: 사용자 계정 잠금 해제
- **4768**: Kerberos TGT 요청
- **4769**: Kerberos 서비스 티켓 요청
- **4771**: Kerberos 사전 인증 실패
- **4776**: NTLM 인증 시도

#### 로그 용량 계산
```
총 용량 = 이벤트 크기 × 이벤트 수 × 보관 일수
예: 500바이트 × 1,000건/일 × 30일 = 15MB
```

### 9.2 파일시스템 및 경로

#### 파일시스템
- **NTFS**: 권한 설정, 암호화, 압축, 대용량 지원
- **FAT32**: 4GB 파일 크기 제한, 권한 설정 불가
- **exFAT**: FAT32 제한 극복, USB용

#### 로그 파일 경로
- **이벤트 로그**: `C:\Windows\System32\winevt\Logs\`
- **IIS 로그**: `C:\inetpub\logs\LogFiles\W3SVC1\`
- **HTTPERR 로그**: `C:\Windows\System32\LogFiles\HTTPERR\`
- **DHCP 로그**: `C:\Windows\System32\LogFiles\DHCP\`
- **SAM 파일**: `C:\Windows\System32\config\SAM` (계정 정보)
- **시스템 레지스트리**: `C:\Windows\System32\config\`

### 9.3 레지스트리 🔴 암기 필수

#### 주요 하이브
- **HKEY_LOCAL_MACHINE (HKLM)**: 시스템 전체 설정
- **HKEY_CURRENT_USER (HKCU)**: 현재 사용자 설정
- **HKEY_CLASSES_ROOT (HKCR)**: 파일 연결 정보
- **HKEY_USERS (HKU)**: 모든 사용자 프로필
- **HKEY_CURRENT_CONFIG**: 현재 하드웨어 프로필

#### 자동 실행 레지스트리 경로
- **Run 키**: 로그인 시 자동 실행
  - `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
  - `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- **RunOnce 키**: 1회만 자동 실행
  - `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
  - `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- **RunServices 키**: 서비스 시작 시 실행
  - `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices`
- **RunMRU 키**: 최근 실행 명령어
  - `HKU\{USER}\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`

### 9.4 Windows 명령어

#### 계정 관리
```cmd
net user                    # 사용자 목록
net user username password /add    # 사용자 추가
net user username /delete   # 사용자 삭제
net user username /active:no       # 계정 비활성화
net localgroup             # 로컬 그룹 목록
net localgroup Administrators      # 관리자 그룹 멤버
net localgroup Administrators username /add   # 그룹 추가
```

#### 프로세스 및 네트워크
```cmd
tasklist                   # 프로세스 목록
tasklist /svc              # 서비스와 함께 표시
taskkill /PID 1234         # PID로 종료
taskkill /IM process.exe /F # 프로세스명으로 강제 종료
netstat -ano               # 네트워크 연결 (PID 포함)
netstat -abn               # 프로세스명 포함 (관리자 권한)
```

#### 서비스 관리
```cmd
sc query                   # 서비스 목록
sc start service           # 서비스 시작
sc stop service            # 서비스 중지
sc config service start=disabled   # 서비스 비활성화
```

#### 방화벽
```cmd
netsh advfirewall show allprofiles        # 방화벽 상태
netsh advfirewall set allprofiles state on    # 방화벽 켜기
netsh advfirewall firewall add rule name="Allow 80" dir=in action=allow protocol=TCP localport=80
```

#### 레지스트리
```cmd
regedit                    # 레지스트리 편집기 (GUI)
reg query HKLM\SOFTWARE    # 레지스트리 조회
reg add "경로" /v 값이름 /t REG_SZ /d 데이터   # 추가
reg delete "경로" /v 값이름   # 삭제
```

### 9.5 Active Directory & GPO

#### Active Directory
- 중앙 집중식 인증 및 권한 관리
- 도메인 컨트롤러 (DC)
- LDAP 프로토콜 사용

#### GPO (Group Policy Object)
- 도메인 정책 관리
- 사용자/컴퓨터 설정 일괄 적용
- `gpupdate /force`: GPO 즉시 적용

### 9.6 BitLocker
- TPM (Trusted Platform Module) 기반
- 전체 디스크 암호화
- 운영체제 볼륨 보호

### 9.7 PE (Portable Executable) 구조
- `.text`: 실행 코드
- `.data`: 초기화된 전역 변수
- `.bss`: 초기화되지 않은 전역 변수
- `.idata`: Import 함수 정보 (DLL)
- `.edata`: Export 함수 정보
- `.rsrc`: 리소스 (아이콘, 문자열)

---

## Part 10. 웹 애플리케이션 보안

### 10.1 웹 취약점 (OWASP Top 10 기반)

#### SQL Injection 🔴 기출 빈출

**공격 기법**
```sql
-- 인증 우회
' OR '1'='1' --
' OR '1'='1' #
admin' --

-- UNION 기반
' UNION SELECT null, username, password FROM users --

-- Blind SQL Injection
' AND 1=1 --  (참)
' AND 1=2 --  (거짓)

-- Time-based Blind
' AND SLEEP(5) --
```

**대응 방법**
- **Prepared Statement** (파라미터화된 쿼리)
- 입력값 검증 및 필터링
- 최소권한 원칙 (DB 계정)
- 에러 메시지 숨김
- WAF (Web Application Firewall)

#### XSS (Cross-Site Scripting)

**Reflected XSS (반사형)**
- URL 파라미터에 스크립트 삽입
- 즉시 실행
- 예: `http://site.com/search?q=<script>alert(1)</script>`

**Stored XSS (저장형)**
- DB에 스크립트 저장
- 다른 사용자가 조회 시 실행
- 위험도 높음

**DOM-based XSS**
- 클라이언트 측 JavaScript에서 발생
- `document.write()`, `innerHTML` 악용

**대응 방법**
- 출력 인코딩 (HTML Entity Encoding)
  - `<` → `&lt;`
  - `>` → `&gt;`
  - `"` → `&quot;`
- CSP (Content-Security-Policy) 헤더
- HttpOnly 쿠키 (JavaScript 접근 차단)

#### CSRF (Cross-Site Request Forgery)

**공격 과정**
1. 사용자가 사이트 A에 로그인
2. 공격자가 악성 사이트 B 유도
3. 사이트 B에서 사이트 A로 요청 전송
4. 사용자 권한으로 의도하지 않은 동작 수행

**대응 방법**
- CSRF Token 검증
- SameSite Cookie 속성
  - `SameSite=Strict`: 동일 사이트만
  - `SameSite=Lax`: GET은 허용
- Referer 검증
- 중요 작업은 재인증 요구

#### Command Injection

**공격 예시**
```bash
; ls -la
| cat /etc/passwd
&& whoami
`cat /etc/shadow`
```

**대응**
- 입력값 검증 (화이트리스트)
- 위험한 함수 사용 금지 (`system()`, `exec()`, `eval()`)
- 최소권한으로 실행

#### Directory Traversal (경로 조작)

**공격 예시**
```
GET /download?file=../../../etc/passwd
GET /download?file=....//....//etc/passwd  (필터 우회)
GET /download?file=%2e%2e%2f%2e%2e%2fetc/passwd  (인코딩)
```

**대응**
- 절대 경로 사용 금지
- 입력값 검증 (`..`, `/` 차단)
- 화이트리스트 방식
- `realpath()` 등으로 정규화

#### XXE (XML External Entity)

**공격 예시**
```xml
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```

**대응**
- 외부 엔티티 비활성화
- XML 파서 보안 설정
- JSON 사용 권장

#### SSRF (Server-Side Request Forgery)

**공격 예시**
```
GET /proxy?url=http://localhost:8080/admin
GET /proxy?url=http://169.254.169.254/latest/meta-data/  (AWS 메타데이터)
```

**대응**
- URL 화이트리스트
- 내부 IP 차단
- 네트워크 분리

#### Insecure Deserialization (역직렬화 취약점)

**위험**
- 원격 코드 실행 (RCE)
- 권한 상승

**대응**
- 신뢰할 수 없는 데이터 역직렬화 금지
- 무결성 검증 (HMAC)
- JSON 등 안전한 포맷 사용

#### 파일 업로드 취약점

**공격 기법**
- 확장자 우회: `shell.php.jpg`, `shell.php%00.jpg`
- Content-Type 변조 (프록시 사용)
- 이중 확장자: `shell.php.png`
- 대소문자: `shell.PhP`

**대응**
- 화이트리스트 확장자 검증
- 파일 내용 검증 (Magic Number)
- 업로드 디렉토리 실행 권한 제거
- 파일명 랜덤화
- `LimitRequestBody`: 최대 파일 크기 제한

#### 추가 웹 공격

**세션 고정 공격 (Session Fixation)**
- 공격자가 세션 ID를 미리 지정
- 로그인 후에도 동일 세션 ID 사용
- **대응**: 로그인 시 세션 ID 재발급

**클릭재킹 (Clickjacking)**
- 투명한 iframe으로 클릭 유도
- **대응**: `X-Frame-Options` 헤더
  - `DENY`: iframe 금지
  - `SAMEORIGIN`: 동일 출처만

**타이밍 공격 (Timing Attack)**
- 응답 시간 차이로 정보 유추
- **대응**: 상수 시간 비교

**HTTP 파라미터 오염 (HPP - HTTP Parameter Pollution)**
- 동일 파라미터 중복 전송
- **대응**: 파라미터 정규화

**HTTP Response Splitting**
- CRLF(`\r\n`) 인젝션
- 헤더 조작
- **대응**: 입력값 검증, CRLF 제거

**Open Redirect**
- 신뢰할 수 없는 URL로 리다이렉트
- 피싱에 악용
- **대응**: URL 화이트리스트

---

### 10.2 웹 서버 보안 설정

#### Apache 보안 설정 🔴 기출 빈출

**httpd.conf 주요 설정**

```apache
# 서버 정보 숨김
ServerTokens Prod           # Apache만 표시 (버전 정보 숨김)
ServerSignature Off         # 오류 페이지 하단 서버 정보 제거

# 디렉토리 인덱싱 차단
<Directory /var/www/html>
    Options -Indexes        # 파일 목록 노출 방지
</Directory>

# 특정 파일 접근 차단
<FilesMatch "\.(bak|config|sql|log)$">
    Require all denied
</FilesMatch>

# 파일 업로드 크기 제한
LimitRequestBody 10485760   # 10MB

# 심볼릭 링크 차단
Options -FollowSymLinks

# CGI 실행 제한
Options -ExecCGI

# .htaccess 사용 제어
AllowOverride None          # .htaccess 비활성화 (성능 향상)
```

**.htaccess 보안 설정**

```apache
# PHP 설정 (실기에서 자주 출제)
php_flag allow_url_fopen Off          # 원격 파일 include 차단
php_flag allow_url_include Off
php_flag display_errors Off           # 에러 표시 Off
php_flag log_errors On                # 에러 로깅 On
php_value error_log /var/log/php_errors.log

# 파일 업로드 제한
php_value upload_max_filesize 5M
php_value post_max_size 10M

# 특정 확장자 실행 차단
<FilesMatch "\.(php|php3|php4|php5|phtml)$">
    Require all denied
</FilesMatch>

# 디렉토리 인덱싱 차단
Options -Indexes
```

#### 보안 헤더 (🔴 중요)

**CSP (Content-Security-Policy)**
```
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.com
```
- XSS 공격 방지
- 리소스 로드 출처 제한

**X-Frame-Options**
```
X-Frame-Options: DENY              # iframe 금지
X-Frame-Options: SAMEORIGIN        # 동일 출처만 허용
```
- 클릭재킹 방지

**HSTS (HTTP Strict Transport Security)**
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```
- HTTPS 강제
- 중간자 공격 방지

**X-Content-Type-Options**
```
X-Content-Type-Options: nosniff
```
- MIME 타입 스니핑 방지

**X-XSS-Protection**
```
X-XSS-Protection: 1; mode=block
```
- 브라우저 XSS 필터 활성화 (구형 브라우저용)

**Referrer-Policy**
```
Referrer-Policy: no-referrer
```
- Referer 헤더 제어

#### 쿠키 보안 속성

```
Set-Cookie: sessionid=abc123; HttpOnly; Secure; SameSite=Strict
```

- **HttpOnly**: JavaScript 접근 차단 (XSS 방지)
- **Secure**: HTTPS만 전송
- **SameSite**: CSRF 방지
  - `Strict`: 동일 사이트만
  - `Lax`: 일부 크로스 사이트 허용
  - `None`: 모든 크로스 사이트 허용 (Secure 필수)

#### 세션 vs 쿠키

| 구분 | 세션 (Session) | 쿠키 (Cookie) |
|------|----------------|---------------|
| 저장 위치 | 서버 | 클라이언트 |
| 보안 | 높음 | 낮음 |
| 용량 제한 | 제한 없음 | 4KB |
| 속도 | 느림 | 빠름 |
| 만료 | 브라우저 종료 시 | 설정 기간 |

#### robots.txt

```
User-agent: *
Disallow: /admin/
Disallow: /private/
```

- 검색엔진 크롤링 제어
- 보안 목적은 아님 (숨김 효과 X)

#### 안전한 코딩 원칙

1. **입력값 검증 (Input Validation)**
   - 화이트리스트 방식
   - 타입, 길이, 형식 검증

2. **출력값 인코딩 (Output Encoding)**
   - HTML Entity Encoding
   - URL Encoding
   - JavaScript Encoding

3. **에러 메시지 최소화**
   - 상세 에러 정보 숨김
   - 일반 메시지만 표시

4. **최소권한 원칙**
   - DB 계정 권한 최소화
   - 파일 권한 최소화

5. **보안 라이브러리 사용**
   - Prepared Statement
   - CSRF Token
   - 검증된 암호화 라이브러리

---

# 🔍 CHECK (점검) - 보안 모니터링

## Part 11. 침입탐지/방지 시스템 (IDS/IPS)

### 11.1 IDS/IPS 개요

#### IDS vs IPS

| 구분 | IDS (Intrusion Detection System) | IPS (Intrusion Prevention System) |
|------|----------------------------------|-----------------------------------|
| 역할 | 탐지 및 경고 | 탐지 및 차단 |
| 위치 | 네트워크 외부 (모니터링) | 네트워크 경로 상 (인라인) |
| 동작 | Passive | Active |
| 오탐 영향 | 경고만 | 정상 트래픽 차단 가능 |

#### 탐지 방식

**시그니처 기반 (Signature-based)**
- 알려진 공격 패턴 매칭
- 장점: 정확도 높음, 오탐 적음
- 단점: 신규 공격 탐지 불가, 시그니처 업데이트 필요

**이상행위 기반 (Anomaly-based)**
- 정상 행위 학습 후 이탈 탐지
- 장점: 신규 공격 탐지 가능
- 단점: 오탐 많음, 학습 필요

#### 배치 방식

**네트워크 기반 (NIDS/NIPS)**
- 네트워크 트래픽 분석
- 예: Snort, Suricata

**호스트 기반 (HIDS/HIPS)**
- 시스템 로그, 파일 무결성 검사
- 예: OSSEC, Tripwire

### 11.2 Snort 🔴 기출 빈출

#### Snort 룰 구조

```
action protocol src_ip src_port direction dst_ip dst_port (rule options)
```

**예시**
```
alert tcp any any -> 192.168.1.0/24 80 (msg:"HTTP GET Request"; content:"GET"; sid:1000001; rev:1;)
```

#### Action 유형
- **alert**: 경고 생성 및 로깅
- **log**: 로깅만
- **pass**: 무시
- **drop**: 차단 (IPS 모드)
- **reject**: 차단 + RST/ICMP 응답

#### 프로토콜
- **tcp**, **udp**, **icmp**, **ip**

#### 방향 지시자
- **->**: 단방향
- **<>**: 양방향

#### SID (Signature ID) 범위 🔴 암기 필수

- **0-99**: 시스템 예약
- **100-999,999**: Snort 공식 룰
- **1,000,000-1,999,999**: Snort 공식 룰 (추가)
- **2,000,000 이상**: 사용자 정의 룰

#### 주요 룰 옵션 🔴 기출 빈출

**메시지 및 식별**
- **msg**: 경고 메시지
  - `msg:"SQL Injection Detected";`
- **sid**: 룰 고유 ID
  - `sid:1000001;`
- **rev**: 룰 버전
  - `rev:1;`
- **classtype**: 공격 분류
  - `classtype:web-application-attack;`
- **priority**: 우선순위 (1-4, 낮을수록 높음)
  - `priority:1;`

**패턴 탐지**
- **content**: 문자열 패턴 매칭
  - `content:"admin";`
  - `content:"|0d 0a|";` (16진수)
- **nocase**: 대소문자 구분 안 함
  - `content:"admin"; nocase;`
- **depth**: 처음부터 N바이트 내 검색
  - `content:"GET"; depth:4;`
- **offset**: N바이트 이후부터 검색
  - `content:"admin"; offset:10;`
- **distance**: 이전 content로부터 상대 위치
  - `content:"password"; distance:0;`
- **within**: 이전 content로부터 N바이트 내
  - `content:"password"; within:20;`

**임계값 (Threshold)**
- **threshold**: 이벤트 발생 조건
  - `threshold: type threshold, track by_src, count 5, seconds 60;`
  - `type`: threshold, limit, both
  - `track`: by_src (출발지), by_dst (목적지)
  - `count`: 횟수
  - `seconds`: 시간

**플래그**
- **flags**: TCP 플래그
  - `flags:S;` (SYN)
  - `flags:SA;` (SYN+ACK)
  - `flags:F;` (FIN)
  - `flags:R;` (RST)

**기타**
- **flow**: 연결 상태
  - `flow:to_server,established;`
  - `flow:from_server;`
- **ttl**: TTL 값
  - `ttl:1;`
- **fragbits**: IP 단편화 비트
  - `fragbits:M;` (More Fragments)

#### Snort 탐지 예제 (기출 유형)

**1. SSH Brute Force 탐지**
```
alert tcp any any -> $HOME_NET 22 (msg:"SSH Brute Force Attack"; \
    threshold:type threshold, track by_src, count 5, seconds 30; \
    sid:1000010; rev:1;)
```
- 30초 내 동일 출발지에서 5회 연결 시 경고

**2. Ping of Death**
```
alert icmp any any -> $HOME_NET any (msg:"Ping of Death"; \
    dsize:>1000; \
    threshold:type threshold, track by_src, count 10, seconds 2; \
    sid:1000020; rev:1;)
```
- 2초 내 1000바이트 초과 ICMP 10개 이상

**3. Port Scan (FIN Scan)**
```
alert tcp any any -> $HOME_NET any (msg:"FIN Scan Detected"; \
    flags:F; \
    threshold:type threshold, track by_src, count 10, seconds 5; \
    sid:1000030; rev:1;)
```
- 5초 내 FIN 플래그 10회 이상

**4. SQL Injection**
```
alert tcp any any -> $HOME_NET 80 (msg:"SQL Injection Attempt"; \
    content:"' OR '1'='1"; nocase; \
    sid:1000040; rev:1;)
```

**5. Heartbleed 공격**
```
alert tcp any any -> $HOME_NET 443 (msg:"Heartbleed Attack"; \
    content:"|18 03|"; depth:2; \
    content:"|01|"; distance:3; within:1; \
    sid:1000050; rev:1;)
```

**6. HTTP GET Flooding**
```
alert tcp any any -> $HOME_NET 80 (msg:"HTTP GET Flooding"; \
    content:"GET"; depth:3; \
    threshold:type threshold, track by_src, count 100, seconds 10; \
    sid:1000060; rev:1;)
```

### 11.3 Suricata

- Snort 호환 + 추가 기능
- 멀티스레드 지원
- IPv6 지원
- GPU 가속
- 대용량 트래픽 처리

---

## Part 12. 보안관제 (SOC/SIEM)

### 12.1 SIEM (Security Information and Event Management)
- 로그 수집 및 통합
- 실시간 상관 분석
- 이상 탐지
- 대시보드 및 리포팅

### 12.2 SOC (Security Operation Center)
- 24/7 보안 모니터링
- 침해사고 대응
- 위협 인텔리전스 활용

### 12.3 취약점 스캐너
- **NESSUS**: 자동화 취약점 스캔
- **Tripwire**: 파일 무결성 점검
- **hping3**: 패킷 생성 및 테스트

---

# ⚡ ACT (개선) - 사고 대응 및 개선

## Part 13. 침해사고 대응

### 13.1 사이버위기 경보 단계
1. **관심 (Blue)**: 정보 수집
2. **주의 (Yellow)**: 상황 주시
3. **경계 (Orange)**: 대응 준비
4. **심각 (Red)**: 즉시 대응

### 13.2 침해사고 대응 7단계
1. **사고 전 준비**: 대응 계획, 팀 구성
2. **사고 탐지**: IDS, 로그 분석
3. **초기 대응**: 격리, 증거 보전
4. **대응 전략 체계화**: 분석 및 대응 방안
5. **사고 조사**: 원인 분석, 피해 범위 파악
6. **보고서 작성**: 경과 및 조치사항 문서화
7. **해결**: 시스템 복구, 재발 방지

### 13.3 공격 분석 프레임워크

#### 사이버 킬체인 (Cyber Kill Chain) - 7단계
1. **정찰 (Reconnaissance)**: 정보 수집
2. **무기화 (Weaponization)**: 악성코드 제작
3. **전달 (Delivery)**: 이메일, USB 등
4. **익스플로잇 (Exploitation)**: 취약점 공격
5. **설치 (Installation)**: 악성코드 설치
6. **C&C (Command and Control)**: 원격 제어
7. **목표 달성 (Actions on Objectives)**: 데이터 탈취

#### MITRE ATT&CK - 14단계 확장 모델
- 초기 접근, 실행, 지속성, 권한 상승, 방어 회피, 자격 증명 접근, 탐색, 측면 이동, 수집, C&C, 유출, 영향

---

## Part 14. 악성코드 및 포렌식

### 14.1 악성코드 분류

#### 바이러스 유형
- **조크 바이러스**: 심리적 동요, 실제 피해 없음
- **파일 감염 바이러스**: 실행 파일 감염
- **부트섹터 바이러스**: MBR 감염
- **매크로 바이러스**: MS Office 매크로 악용

#### 기타 악성코드
- **웜**: 자가 복제 및 전파 (네트워크)
- **트로이목마**: 정상 프로그램 위장
- **랜섬웨어**: 파일 암호화 후 금전 요구
- **크립토재킹**: 암호화폐 채굴
- **APT (Advanced Persistent Threat)**: 지속적 표적 공격
- **Zero-day exploit**: 패치 전 취약점 공격
- **루트킷**: 관리자 권한 획득 후 은폐
- **스파이웨어**: 정보 수집 및 전송
- **애드웨어**: 광고 표시

### 14.2 악성코드 분석

#### 분석 도구
- **YARA**: 패턴 기반 악성코드 분류
- **VirusTotal**: 온라인 멀티 엔진 스캔
- **IDA Pro**: 디스어셈블러
- **OllyDbg**: 디버거

#### 분석 방법
- **정적 분석**: 실행 없이 코드 분석 (문자열, PE 구조)
- **동적 분석**: 샌드박스에서 실행 관찰 (행위 분석)
- **샌드박스**: 격리된 환경에서 실행

### 14.3 디지털 포렌식 🔴 기출 빈출

#### 디지털 증거 수집 5원칙
1. **정당성의 원칙**: 적법한 절차
2. **재현의 원칙**: 동일한 결과 재현 가능
3. **신속성의 원칙**: 빠른 수집
4. **연계 보관성의 원칙**: Chain of Custody
5. **무결성의 원칙**: 원본 변조 방지

#### Chain of Custody (증거 보관 연속성)
- 증거 수집부터 법정 제출까지 모든 과정 기록
- 누가, 언제, 어디서, 무엇을, 왜

#### 해시 검증
- **MD5**: 128비트 (충돌 발견, 비권장)
- **SHA-1**: 160비트 (충돌 발견, 비권장)
- **SHA-256**: 256비트 (권장)
- 증거 무결성 입증

#### 휘발성 순서 (Order of Volatility)
1. **레지스터, 캐시**: 가장 휘발성 높음
2. **라우팅 테이블, ARP 캐시, 프로세스 테이블**
3. **메모리 (RAM)**
4. **임시 파일 시스템, 스왑 파일**
5. **디스크**
6. **원격 로깅 데이터, 모니터링 데이터**
7. **물리적 구성, 네트워크 토폴로지**: 가장 휘발성 낮음

#### 포렌식 유형
- **디스크 포렌식**: 파일 시스템 분석
- **메모리 포렌식**: RAM 덤프 분석
- **네트워크 포렌식**: 패킷 캡처 분석
- **모바일 포렌식**: 스마트폰 데이터 분석

#### 포렌식 도구
- **Volatility**: 메모리 포렌식
- **FTK (Forensic Toolkit)**: 디스크 포렌식
- **EnCase**: 종합 포렌식
- **Autopsy**: 오픈소스 디지털 포렌식
- **Wireshark**: 네트워크 패킷 분석
- **dd**: 디스크 이미지 생성 (`dd if=/dev/sda of=image.dd`)

#### 파일 시스템 분석
- **MFT (Master File Table)**: NTFS 파일 시스템 메타데이터
- **FAT (File Allocation Table)**: FAT32 파일 시스템
- **Slack Space**: 파일 끝과 클러스터 끝 사이 공간 (숨김 데이터 가능)
- **Unallocated Space**: 삭제된 파일 영역

#### 안티포렌식 기법
- 데이터 삭제 및 와이핑
- 암호화
- 스테가노그래피 (데이터 은닉)
- 타임스탬프 변조
- 로그 삭제

### 14.4 사회공학 기법
- **피싱 (Phishing)**: 이메일 사칭
- **스피어 피싱 (Spear Phishing)**: 특정 대상 공격
- **웨일링 (Whaling)**: 고위 임원 대상
- **스미싱 (Smishing)**: SMS 피싱
- **큐싱 (Qshing)**: QR 코드 악용
- **파밍 (Pharming)**: DNS 변조로 가짜 사이트 유도

---

## Part 15. 법령 및 규제

### 15.1 개인정보보호법 🔴 기출 필수

#### 주요 조항
- **제15조**: 개인정보 수집·이용 동의
- **제17조**: 개인정보 제3자 제공
- **제25조**: CCTV 설치·운영 제한
- **제29조**: 안전성 확보조치 의무

#### 안전성 확보조치 기준
- **접속기록 보존**: 1년 이상 (5만명 이상 또는 고유식별정보/민감정보는 2년)
- **접속기록 점검**: 월 1회 이상
- **권한 부여/변경/말소 기록**: 3년 보관

#### 암호화 대상 및 방법 🔴 암기 필수

**일방향 암호화 (해시)**
- **대상**: 비밀번호, 바이오정보
- **방법**: SHA-256 이상

**양방향 암호화 (대칭키)**
- **대상**:
  - 주민등록번호, 여권번호, 운전면허번호, 외국인등록번호
  - 신용카드번호, 계좌번호
- **방법**: AES, SEED, ARIA 등

#### 개인정보 제3자 제공 고지사항
1. 제공받는 자
2. 이용 목적
3. 제공 항목
4. 보유/이용 기간
5. 동의거부 권리 및 불이익

#### 개인정보 유출 통지
- 유출 인지 시 72시간 내 정보주체 통지
- 개인정보보호위원회 신고

### 15.2 정보통신망법

#### 정보통신망 정의
- 전기통신설비 + 컴퓨터 및 이용기술

#### 정보통신서비스 제공자 의무
- 개인정보 암호화
- 접근제어
- 로그 보관

#### 정보통신기반시설
- 국가안전보장·행정·국방·치안·금융·통신·운송·에너지 관련 시스템
- 주요정보통신기반시설 지정 및 보호

### 15.3 기타 법령
- **전자금융감독규정**: 금융 시스템 보안
- **국가정보보안기본지침**: 공공기관 보안
- **전자서명법**: 전자서명 및 인증서
- **정보통신기반보호법**: 주요 정보통신기반시설 보호
- **개인정보 영향평가 (PIA)**: 대규모 개인정보 처리 시 사전 평가

---

## Part 16. 백업 및 재해복구

### 16.1 백업 전략

#### 백업 유형 🔴 기출 빈출
- **전체 백업 (Full Backup)**
  - 모든 데이터 백업
  - 복구 시간 짧음, 저장공간 많이 필요
  - 주 1회

- **증분 백업 (Incremental Backup)**
  - 마지막 백업 이후 변경된 데이터만
  - 저장공간 적음, 복구 시간 김 (모든 증분 필요)
  - 매일

- **차등 백업 (Differential Backup)**
  - 마지막 전체 백업 이후 변경된 데이터
  - 증분과 전체의 중간
  - 복구: 전체 + 마지막 차등

#### 백업 전략
- **3-2-1 규칙**:
  - 3개 복사본
  - 2개 다른 매체 (HDD, 테이프)
  - 1개 오프사이트 (원격지)

### 16.2 재해복구

#### 핵심 지표 🔴 암기 필수
- **RTO (Recovery Time Objective)**: 목표 복구 시간
  - 시스템 중단 후 복구까지 허용 시간
  - 예: 4시간

- **RPO (Recovery Point Objective)**: 목표 복구 시점
  - 데이터 손실 허용 범위
  - 예: 1시간 (1시간 전 데이터까지 복구)

#### 업무연속성
- **BCP (Business Continuity Plan)**: 업무연속성 계획
- **DRP (Disaster Recovery Plan)**: 재해복구 계획

#### 재해복구 센터
- **Hot Site**: 즉시 전환 가능 (실시간 동기화, 비용 高)
- **Warm Site**: 수 시간~일 내 전환 (주기적 백업)
- **Cold Site**: 장비만 준비, 설치 필요 (비용 低)

---

## Part 17. 최신 보안 트렌드 (KISA 가이드라인 반영)

### 17.1 Zero Trust 보안 모델
- "신뢰하지 말고 항상 검증"
- 모든 접근 요청 검증
- 최소 권한 접근
- 마이크로 세그멘테이션

### 17.2 클라우드 보안

#### 서비스 모델별 책임 범위
- **IaaS**: OS, DB, 미들웨어, 애플리케이션 직접 관리
- **PaaS**: 애플리케이션만 관리
- **SaaS**: 계정/권한만 관리

#### 공유 책임 모델
- 클라우드 제공자: 인프라 보안
- 사용자: 데이터 및 애플리케이션 보안

### 17.3 컨테이너 및 DevSecOps
- **Docker 보안**: 이미지 검증, 최소 권한
- **Kubernetes 보안**: RBAC, Network Policy
- **컨테이너 이미지 취약점 스캔**

### 17.4 AI/ML 보안
- 적대적 공격 (Adversarial Attack)
- 모델 추출 공격
- 데이터 포이즈닝

### 17.5 IoT 보안
- 펌웨어 보안
- 디폴트 패스워드 변경
- 보안 업데이트

---

# 📌 실기 시험 핵심 체크리스트

## 🔴 최우선 암기 (기출 빈출 Top 20)

### 1. 포트번호 완전 정복
```
FTP: 20(data), 21(control)
SSH: 22
Telnet: 23
SMTP: 25
DNS: 53 (TCP/UDP)
DHCP: 67(server), 68(client)
TFTP: 69
HTTP: 80
POP3: 110
NTP: 123
IMAP: 143
SNMP: 161(agent), 162(manager)
LDAP: 389
HTTPS: 443
SMB: 445
Syslog: 514
MSSQL: 1433
Oracle: 1521
RADIUS: 1812(auth), 1813(accounting)
L2TP: 1701
PPTP: 1723
SSDP: 1900
MySQL: 3306
RDP: 3389
```

### 2. 암호화 알고리즘 스펙
```
DES: 56비트 키, 64비트 블록, 16라운드, Feistel
3DES: 168비트 키 (56×3)
AES: 128/192/256비트 키, 128비트 블록, 10/12/14라운드
SEED: 128비트 키/블록, 16라운드, Feistel
ARIA: 128/192/256비트 키, 128비트 블록
```

### 3. Linux 파일 경로
```
/etc/passwd              # 사용자 계정
/etc/shadow              # 암호화된 패스워드 (root만)
/etc/group               # 그룹 정보
/etc/login.defs          # 패스워드 정책
/var/log/secure          # 인증 로그 (su, SSH, 원격)
/var/log/messages        # 시스템 로그
/var/log/wtmp            # 로그인 기록
/var/log/btmp            # 실패한 로그인
/etc/ssh/sshd_config     # SSH 설정
/etc/hosts.allow         # tcpwrapper 허용
/etc/hosts.deny          # tcpwrapper 거부
```

### 4. Windows 이벤트 ID
```
4624: 로그온 성공
4625: 로그온 실패
4672: 관리자 권한 로그온
4720: 사용자 계정 생성
4728: 보안 그룹에 사용자 추가
4740: 사용자 계정 잠금
```

### 5. Snort SID 범위
```
0-99: 시스템 예약
100-999,999: Snort 공식 룰
1,000,000 이상: 사용자 정의 룰
```

### 6. IPSec
```
AH: 무결성, 인증 (암호화 X), 프로토콜 51
ESP: 기밀성, 무결성, 인증, 프로토콜 50
전송모드: 페이로드만 보호 (호스트 간)
터널모드: 전체 패킷 보호 + 새 IP 헤더 (VPN)
```

### 7. 법령 암호화 기준
```
일방향 (해시): 비밀번호, 바이오정보 → SHA-256 이상
양방향 (대칭키): 주민번호, 신용카드번호 등 → AES, SEED, ARIA
```

### 8. 안전성 확보조치
```
접속기록 보존: 1년 이상 (5만명 이상은 2년)
접속기록 점검: 월 1회 이상
권한 기록 보관: 3년
```

### 9. 백업 및 재해복구
```
Full Backup: 모든 데이터
Incremental: 마지막 백업 이후 변경분
Differential: 마지막 전체 백업 이후 변경분

RTO: 목표 복구 시간
RPO: 목표 복구 시점 (데이터 손실 허용 범위)
```

### 10. 포렌식 5원칙
```
1. 정당성
2. 재현성
3. 신속성
4. 연계 보관성 (Chain of Custody)
5. 무결성
```

## 🟡 중요 명령어 정리

### Linux
```bash
# 로그 조회
last                 # 로그인 기록
lastb                # 실패한 로그인
lastlog              # 마지막 로그인
lastcomm             # 프로세스 실행 기록

# 프로세스
ps -ef               # 프로세스 목록
top                  # 실시간 모니터링
kill -9 PID          # 강제 종료

# 네트워크
netstat -ano         # 네트워크 연결
ss -tuln             # 리스닝 포트 (netstat 대체)
lsof -i :80          # 80번 포트 사용 프로세스
tcpdump -i eth0      # 패킷 캡처
arp -a               # ARP 캐시

# 파일 권한
chmod 755 file       # rwxr-xr-x
chmod u+s file       # SetUID
chmod g+s file       # SetGID
chmod +t dir         # Sticky Bit
umask 022            # 기본 권한 마스크

# 방화벽
iptables -L          # 규칙 목록
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### Windows
```cmd
net user                           # 사용자 목록
net user username password /add    # 사용자 추가
net localgroup Administrators      # 관리자 그룹
tasklist                           # 프로세스 목록
taskkill /PID 1234                # 프로세스 종료
netstat -ano                       # 네트워크 연결
sc query                           # 서비스 목록
```

### nmap
```bash
-sT    # TCP Connect 스캔
-sS    # SYN 스캔 (Stealth)
-sU    # UDP 스캔
-sN    # Null 스캔
-sF    # FIN 스캔
-sX    # XMAS 스캔
-O     # OS 탐지
-sV    # 버전 탐지
-A     # Aggressive (종합 스캔)
-p     # 포트 지정
```

## 🟢 약어 풀네임 (필수 암기)

### 보안 모델/체계
```
CIA: Confidentiality, Integrity, Availability
AAA: Authentication, Authorization, Accounting
ISMS-P: Information Security Management System - Personal information
DAC: Discretionary Access Control
MAC: Mandatory Access Control
RBAC: Role-Based Access Control
```

### 암호화
```
AES: Advanced Encryption Standard
DES: Data Encryption Standard
RSA: Rivest-Shamir-Adleman
ECC: Elliptic Curve Cryptography
HMAC: Hash-based Message Authentication Code
PBKDF2: Password-Based Key Derivation Function 2
```

### 네트워크
```
ARP: Address Resolution Protocol
DHCP: Dynamic Host Configuration Protocol
DNS: Domain Name System
NAT: Network Address Translation
PAT: Port Address Translation
VPN: Virtual Private Network
VLAN: Virtual Local Area Network
IPSec: IP Security
TLS: Transport Layer Security
SSL: Secure Sockets Layer
```

### 공격 기법
```
XSS: Cross-Site Scripting
CSRF: Cross-Site Request Forgery
SSRF: Server-Side Request Forgery
XXE: XML External Entity
DoS: Denial of Service
DDoS: Distributed Denial of Service
MITM: Man-in-the-Middle
```

### 보안 솔루션
```
IDS: Intrusion Detection System
IPS: Intrusion Prevention System
WAF: Web Application Firewall
SIEM: Security Information and Event Management
SOC: Security Operation Center
CERT: Computer Emergency Response Team
EDR: Endpoint Detection and Response
DLP: Data Loss Prevention
UTM: Unified Threat Management
NAC: Network Access Control
```

### 재해복구
```
BCP: Business Continuity Plan
DRP: Disaster Recovery Plan
RTO: Recovery Time Objective
RPO: Recovery Point Objective
```

### 인증/표준
```
CA: Certificate Authority
PKI: Public Key Infrastructure
CRL: Certificate Revocation List
OCSP: Online Certificate Status Protocol
CC: Common Criteria
EAL: Evaluation Assurance Level
CVE: Common Vulnerabilities and Exposures
CVSS: Common Vulnerability Scoring System
NVD: National Vulnerability Database
```

---

## ✅ 실기 시험 최종 점검 사항

### 시험 1일 전
1. [ ] 포트번호 25개 암기 확인
2. [ ] 암호화 알고리즘 스펙 암기
3. [ ] Linux 파일 경로 10개 암기
4. [ ] Windows 이벤트 ID 6개 암기
5. [ ] Snort SID 범위 암기
6. [ ] IPSec 모드 차이 이해
7. [ ] 법령 암호화 기준 암기
8. [ ] 백업 유형 및 RTO/RPO 이해
9. [ ] 포렌식 5원칙 암기
10. [ ] 주요 명령어 실습

### 시험 당일
- 문제를 정확히 읽기 (요구사항 확인)
- 단위 주의 (MB vs GB, 일 vs 월)
- 경로는 정확하게 (/etc/shadow vs /etc/passwd)
- 명령어 옵션 확인 (대소문자 구분)
- 계산 문제는 검산
- 시간 배분 (어려운 문제는 나중에)

---

# 🎯 학습 완료!

이제 PDCA 사이클을 따라 다시 한번 복습하세요:
1. **Plan**: 보안 체계 및 정책 이해
2. **Do**: 암호화, 시스템, 네트워크, 웹 보안 구현
3. **Check**: IDS/IPS, 로그 분석으로 점검
4. **Act**: 침해사고 대응, 포렌식, 법령 준수

**정보보안기사 실기 합격을 응원합니다! 🎉**
