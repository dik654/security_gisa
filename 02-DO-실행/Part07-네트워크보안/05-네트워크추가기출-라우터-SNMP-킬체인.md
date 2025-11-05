# 네트워크 추가 기출 - 라우터, SNMP, 킬체인

## 🔴 라우터 Telnet 설정 ★★★

### 문제 (2024년 기출)
```
라우터 내 계정으로 telnet 접속 가능하게 만드는 명령어는?
```

### ✅ 정답
```
Router#en
Router(config)#username admin password admin
Router(config-line)#login local
Router(config-line)#end
Router#write memory
```

### 상세 설명
```
1. 특권 모드 진입
Router> en
Router#

2. 전역 설정 모드
Router# configure terminal
Router(config)#

3. 사용자 계정 생성 ★★★
Router(config)# username admin password admin
Router(config)# username admin privilege 15 secret cisco123

4. VTY 라인 설정 (Telnet/SSH)
Router(config)# line vty 0 4
Router(config-line)# login local  ← 로컬 계정 인증 ★★★
Router(config-line)# transport input telnet
Router(config-line)# exit

5. 설정 저장
Router(config)# end
Router# write memory
또는
Router# copy running-config startup-config
```

**추가 명령:**
```
# TCP keepalives 설정 (네트워크로 들어오는)
Router(config-line)# service tcp-keepalives-in ★★

# 비밀번호만 사용 (덜 안전)
Router(config-line)# password cisco
Router(config-line)# login

# SSH 사용 (권장) ★★★
Router(config-line)# transport input ssh
```

---

## 🔴 SNMP ACL ★★

### 문제 (2024년 기출)
```
SNMP에 ACL을 안 걸면 무슨 일이 발생하는가? (틀린 것 고르기)
```

### ✅ 정답 (발생 가능한 문제)
```
1. 무단 접근으로 시스템 정보 유출 ★★★
2. 설정 변경 (SNMP v2c/v3 write community)
3. 서비스 거부 공격
4. 네트워크 트래픽 모니터링 노출
```

### 상세 설명

**SNMP ACL 설정:**
```
# Cisco
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)# snmp-server community public RO 10  ← ACL 10 적용 ★★★
Router(config)# snmp-server community private RW 10
```

**위험:**
- default community (public/private) 사용 시 취약
- ACL 없으면 누구나 접근 가능
- MIB 정보 유출 (시스템 정보, 인터페이스, 라우팅 테이블)

**방어:**
- SNMPv3 사용 (암호화, 인증) ★★★
- Community string 변경
- ACL로 접근 제한

---

## 🔴 IP 클래스 ★★

### 문제 (2024년 기출)
```
IP 클래스에 대해 틀린 것은?
B 클래스: 191.0.0.0 ~ 191.255.255.255 ← 틀림!
```

### ✅ 정답
```
B 클래스: 128.0.0.0 ~ 191.255.255.255 ★★★

191.0.0.0은 B 클래스 범위 내이므로
"B 클래스가 191.x.x.x까지"라는 설명은 맞지만,
"191.0.0.0부터 시작"은 틀림!
```

**IP 클래스 정리:**
```
Class A: 1.0.0.0 ~ 126.255.255.255
         첫 비트: 0
         Default Mask: /8 (255.0.0.0)

Class B: 128.0.0.0 ~ 191.255.255.255 ★★★
         첫 2비트: 10
         Default Mask: /16 (255.255.0.0)

Class C: 192.0.0.0 ~ 223.255.255.255
         첫 3비트: 110
         Default Mask: /24 (255.255.255.0)

Class D: 224.0.0.0 ~ 239.255.255.255 (멀티캐스트)
Class E: 240.0.0.0 ~ 255.255.255.255 (예약)

특수:
127.0.0.0 ~ 127.255.255.255: Loopback
10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16: 사설 IP
```

---

## 🔴 스니핑 공격 ★★

### 문제 (2024년 기출)
```
다음 중 스니핑이 아닌 공격은?
1) 스위치 재밍
2) SPAN
3) ICMP 리다이렉트
4) DNS 공격 ← 스니핑 아님! ★★★
```

### ✅ 정답
```
DNS 공격은 스니핑이 아닌 별도 공격

스니핑 공격:
- 스위치 재밍 (MAC Flooding)
- SPAN (Port Mirroring 악용)
- ICMP 리다이렉트
- ARP Spoofing
```

**스위치 재밍 (MAC Flooding):**
```
스위치의 MAC 테이블을 가짜 MAC으로 채움
→ 테이블 오버플로우
→ 스위치가 허브처럼 동작 (브로드캐스트)
→ 모든 패킷 스니핑 가능 ★★★

방어: Port Security
```

**SPAN (Switched Port Analyzer):**
```
정상 기능: 트래픽 미러링 (모니터링 목적)
악용: 무단으로 SPAN 설정하여 트래픽 복사

방어: SPAN 설정 권한 제한
```

**ICMP 리다이렉트:**
```
ICMP Redirect 메시지로 라우팅 경로 변경
→ 공격자를 거쳐가도록 유도
→ 중간자 공격 + 스니핑

방어: ICMP Redirect 비활성화
```

---

## 🔴 사이버 킬체인 (Cyber Kill Chain) ★★★

### 문제 (2024년 기출)
```
사이버 킬체인 단계 빈칸 맞추기
```

### ✅ 정답
```
Cyber Kill Chain (록히드 마틴)

7단계: ★★★

1. 정찰 (Reconnaissance)
   - 타겟 정보 수집
   - OSINT, 소셜 미디어

2. 무기화 (Weaponization)
   - 익스플로잇 + 악성코드 결합
   - 악성 문서, 첨부파일 제작

3. 전달 (Delivery) ★★
   - 피싱 이메일
   - 워터링 홀
   - USB 드롭

4. 악용 (Exploitation)
   - 취약점 공격
   - 제로데이 악용

5. 설치 (Installation)
   - 백도어 설치
   - 지속성 확보 (레지스트리, 서비스)

6. C&C (Command and Control) ★★★
   - 공격자 서버 연결
   - 명령 수신 및 제어

7. 목표 달성 (Actions on Objectives)
   - 데이터 탈취
   - 시스템 파괴
   - 랜섬웨어 실행
```

**각 단계별 방어:**
```
1. 정찰: 정보 노출 최소화, OSINT 모니터링
2. 무기화: 위협 인텔리전스
3. 전달: 이메일 필터링, 웹 필터링 ★★
4. 악용: 패치, IPS
5. 설치: EDR, 화이트리스팅
6. C&C: 방화벽, DNS 싱크홀 ★★★
7. 목표: DLP, 백업
```

**실기 답안 작성:**
```
정찰 → 무기화 → 전달 → 악용 → 설치 → C&C → 목표달성

또는 영어로:
Reconnaissance → Weaponization → Delivery → Exploitation
→ Installation → C&C → Actions on Objectives
```

---

## 🔴 공격 기법 분류 ★★

### 문제 (2024년 기출)
```
공격기법 - 형태 - 목표가 바르게 짝지어진 것은?
```

### ✅ 정답
```
┌──────────────────┬──────┬────────┐
│   공격 기법      │ 형태 │  목표  │
├──────────────────┼──────┼────────┤
│ Packet Sniffing  │수동적│ 기밀성 │ ★★★
│ (패킷 스니핑)    │      │        │
├──────────────────┼──────┼────────┤
│ Session Hijacking│공격적│ 무결성 │ ★★
│ (세션 하이재킹)  │      │  or    │
│                  │      │ 기밀성 │
├──────────────────┼──────┼────────┤
│ UDP Flood        │공격적│ 가용성 │ ★★★
│ (DoS)            │      │        │
├──────────────────┼──────┼────────┤
│ SQL Injection    │공격적│ 무결성 │ ★★
│                  │      │ 기밀성 │
├──────────────────┼──────┼────────┤
│ ARP Spoofing     │공격적│ 무결성 │ ★★
│                  │      │        │
└──────────────────┴──────┴────────┘

수동적 공격:
- 스니핑, 트래픽 분석
- 목표: 기밀성 침해

공격적 (능동적) 공격:
- DoS, 변조, 삭제
- 목표: 무결성, 가용성 침해
```

---

## 🎯 핵심 암기 카드

### 카드 1: 라우터 Telnet
```
username admin password admin
line vty 0 4
login local ★★★
transport input telnet
```

### 카드 2: SNMP ACL
```
ACL로 접근 제한 필수
SNMPv3 사용 (암호화) ★★★
default community 변경
```

### 카드 3: IP 클래스
```
A: 1.0.0.0 ~ 126.255.255.255
B: 128.0.0.0 ~ 191.255.255.255 ★★★
C: 192.0.0.0 ~ 223.255.255.255
```

### 카드 4: 킬체인
```
정찰 → 무기화 → 전달 → 악용 →
설치 → C&C → 목표달성 ★★★
```

### 카드 5: 공격 분류
```
수동적: 스니핑 → 기밀성 ★★★
공격적: DoS → 가용성 ★★★
공격적: 변조 → 무결성
```

---

## 🔥 실기 시험 최빈출

```
1위: 사이버 킬체인 (7단계 순서)     ← 2024년 출제! ★★★
2위: 라우터 Telnet 설정 (login local) ← 2024년 출제!
3위: IP 클래스 B (128~191)         ← 2024년 출제!
4위: 공격 분류 (수동/공격적, CIA)   ← 2024년 출제!
5위: SNMP ACL                      ← 2024년 출제!
```

**✅ 킬체인은 C&C 단계가 핵심! DNS 싱크홀로 차단!**
**✅ 라우터는 login local로 로컬 계정 인증!**
**✅ 스니핑은 수동적 공격, 기밀성 침해!**
