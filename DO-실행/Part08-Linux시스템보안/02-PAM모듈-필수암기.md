# PAM (Pluggable Authentication Modules) - 필수 암기

## 🔴 PAM 모듈 4가지 유형 (최빈출!)

```
┌──────────┬─────────────────────┬────────────────────┐
│ 모듈유형 │      영문명          │      기능          │
├──────────┼─────────────────────┼────────────────────┤
│ auth     │ Authentication      │ 사용자 인증        │
│          │                     │ - 패스워드 검증    │
│          │                     │ - 생체인증 등      │
├──────────┼─────────────────────┼────────────────────┤
│ account  │ Account Management  │ 계정/권한 확인     │
│          │                     │ - 계정 유효성      │
│          │                     │ - 접근 권한        │
│          │                     │ - 시간/위치 제한   │
├──────────┼─────────────────────┼────────────────────┤
│ session  │ Session Management  │ 세션 설정          │
│          │                     │ - 로그인 전/후 작업│
│          │                     │ - 홈 디렉토리 마운트│
│          │                     │ - 로그 기록        │
├──────────┼─────────────────────┼────────────────────┤
│ password │ Password Management │ 패스워드 관리      │
│          │                     │ - 암호 변경        │
│          │                     │ - 복잡성 검사      │
└──────────┴─────────────────────┴────────────────────┘
```

**★ 암기법: "인계세패" (인증-계정-세션-패스워드)**

---

## 📝 기출 유형 1: PAM 모듈 유형 매칭 ★★★

### 문제 1-1 (2024년 실기)
```
PAM 모듈의 기능 중 아래 설명에 해당하는 모듈 유형을 각각 쓰시오.

ㄱ. 계정 인증
ㄴ. 권한 확인
ㄷ. 세션 연결
```

**✅ 정답:**
```
ㄱ. auth
ㄴ. account
ㄷ. session
```

**❌ 오답 예시:**
```
ㄱ. auth ✓
ㄴ. permission ✗ (PAM 모듈 유형이 아님!)
ㄷ. session ✓
```

---

## 📝 기출 유형 2: PAM 설정 파일 ★★

### PAM 설정 파일 위치
```
/etc/pam.d/          - 서비스별 PAM 설정 디렉토리
/etc/pam.d/sshd      - SSH 서비스 PAM 설정
/etc/pam.d/login     - 로그인 PAM 설정
/etc/pam.d/su        - su 명령어 PAM 설정
/etc/pam.d/sudo      - sudo 명령어 PAM 설정
/etc/pam.d/system-auth - 시스템 인증 공통 설정
/etc/pam.d/password-auth - 패스워드 인증 설정
```

### PAM 설정 파일 형식
```
[모듈유형] [제어플래그] [모듈경로] [모듈옵션]

예시:
auth       required     pam_unix.so
account    sufficient   pam_localuser.so
session    optional     pam_umask.so
password   requisite    pam_pwquality.so retry=3
```

**제어 플래그 (Control Flag):**
```
required   - 반드시 성공해야 함 (실패 시에도 계속 진행)
requisite  - 반드시 성공해야 함 (실패 시 즉시 중단)
sufficient - 성공 시 이후 검사 생략
optional   - 선택 사항 (결과에 영향 없음)
```

---

### 문제 2-1 (PAM 설정)
```
SSH 로그인 시 5회 실패하면 계정을 잠그는 PAM 설정을 작성하시오.
파일: /etc/pam.d/sshd
```

**✅ 답안:**
```
/etc/pam.d/sshd 파일에 추가:

auth required pam_tally2.so deny=5 unlock_time=300

설명:
- auth: 인증 모듈
- required: 반드시 검사
- pam_tally2.so: 로그인 실패 카운트 모듈
- deny=5: 5회 실패 시 잠금
- unlock_time=300: 300초(5분) 후 자동 해제

확인 명령어:
pam_tally2 --user=사용자명

잠금 해제:
pam_tally2 --user=사용자명 --reset
```

---

## 📝 기출 유형 3: 패스워드 복잡성 설정 ★★★

### 문제 3-1 (2023년 실기)
```
패스워드 최소 길이 8자, 대문자·소문자·숫자·특수문자 각 1개 이상 포함하도록
PAM 설정을 작성하시오.
```

**✅ 답안:**
```
/etc/pam.d/system-auth 또는 /etc/pam.d/password-auth 파일 수정:

password requisite pam_pwquality.so retry=3 minlen=8 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1

옵션 설명:
- retry=3: 3회 재시도 허용
- minlen=8: 최소 길이 8자
- dcredit=-1: 숫자 최소 1개 (digit)
- ucredit=-1: 대문자 최소 1개 (uppercase)
- lcredit=-1: 소문자 최소 1개 (lowercase)
- ocredit=-1: 특수문자 최소 1개 (other)

주의:
dcredit=-1 → 최소 1개 필수
dcredit=1  → 있으면 길이 1 감소 (선택)
```

---

## 📝 기출 유형 4: PAM 모듈별 실전 예시

### auth 모듈 예시
```
# 패스워드 인증
auth required pam_unix.so

# 계정 잠금
auth required pam_tally2.so deny=5 unlock_time=600

# Kerberos 인증
auth sufficient pam_krb5.so
```

### account 모듈 예시
```
# 계정 유효성 확인
account required pam_unix.so

# 시간 기반 접근 제어
account required pam_time.so

# 로컬 사용자 확인
account sufficient pam_localuser.so
```

### session 모듈 예시
```
# 세션 시작/종료 로깅
session required pam_unix.so

# 환경 변수 설정
session required pam_env.so

# umask 설정
session optional pam_umask.so

# 로그인 메시지 표시
session optional pam_motd.so
```

### password 모듈 예시
```
# 패스워드 복잡성 검사
password requisite pam_pwquality.so retry=3

# 패스워드 암호화 및 저장
password sufficient pam_unix.so sha512 shadow

# 패스워드 히스토리 (이전 5개 재사용 금지)
password required pam_pwhistory.so remember=5
```

---

## 🎯 PAM 핵심 암기 카드

### 카드 1: 4대 모듈 유형 (최빈출!)
```
auth     - 인증 (패스워드 검증)
account  - 계정/권한 확인
session  - 세션 설정
password - 패스워드 관리

★ "인계세패" 로 암기!
```

### 카드 2: 제어 플래그
```
required   - 필수 (실패해도 계속)
requisite  - 필수 (실패 시 즉시 중단)
sufficient - 성공 시 이후 생략
optional   - 선택 사항
```

### 카드 3: 주요 PAM 모듈
```
pam_unix.so      - 기본 인증 (패스워드)
pam_tally2.so    - 로그인 실패 카운트
pam_pwquality.so - 패스워드 복잡성
pam_limits.so    - 자원 제한
pam_time.so      - 시간 기반 접근 제어
```

### 카드 4: 패스워드 복잡성 옵션
```
minlen   - 최소 길이
dcredit  - 숫자 (digit)
ucredit  - 대문자 (uppercase)
lcredit  - 소문자 (lowercase)
ocredit  - 특수문자 (other)

-1: 최소 1개 필수
+1: 있으면 길이 1 감소
```

---

## 📊 그림으로 이해하기

### PAM 인증 프로세스
```
[사용자 로그인 시도]
        ↓
   ┌─────────────┐
   │ auth 모듈   │ → 패스워드 검증
   └─────────────┘
        ↓ 성공
   ┌─────────────┐
   │ account모듈 │ → 계정 유효성, 권한 확인
   └─────────────┘
        ↓ 성공
   ┌─────────────┐
   │session 모듈 │ → 세션 시작, 로그 기록
   └─────────────┘
        ↓
   [로그인 완료]

[패스워드 변경 시]
        ↓
   ┌─────────────┐
   │password모듈 │ → 복잡성 검사, 암호화 저장
   └─────────────┘
```

### PAM 계정 잠금 동작
```
[로그인 시도]
        ↓
  pam_tally2.so (deny=5)
        ↓
    실패 카운트 확인
        ↓
    ┌─────┬─────┐
    │ <5회│ ≥5회│
    └─────┴─────┘
       ↓      ↓
    진행    차단
            ↓
       [계정 잠김]
            ↓
    unlock_time 경과 후 해제
    또는 관리자가 수동 해제
    (pam_tally2 --reset)
```

---

## 💡 실기 답안 작성 팁

### Tip 1: 모듈 유형 정확히
```
✅ account (권한 확인)
❌ permission (PAM 모듈 아님!)

✅ auth (인증)
❌ authentication (약자로 써야 함)
```

### Tip 2: 설정 파일 경로
```
✅ /etc/pam.d/sshd
✅ /etc/pam.d/system-auth

❌ /etc/pam.conf (구식, 사용 안 함)
```

### Tip 3: 옵션 형식
```
✅ deny=5 (=로 연결, 공백 없음)
❌ deny = 5 (공백 있으면 오류)

✅ dcredit=-1 (음수: 필수)
❌ dcredit=1 (양수: 선택)
```

### Tip 4: 제어 플래그
```
required vs requisite 차이:

required   - 실패해도 모든 모듈 실행 후 거부
requisite  - 실패 시 즉시 거부 (더 안전)

패스워드 복잡성: requisite 권장
```

---

## 📝 종합 문제: PAM 보안 강화

### 종합 문제 1
```
다음 요구사항을 만족하는 PAM 설정을 작성하시오.

1) SSH 로그인 5회 실패 시 10분간 계정 잠금
2) 패스워드 최소 10자, 영문+숫자+특수문자 필수
3) 이전 5개 패스워드 재사용 금지
4) 세션 시작 시 /var/log/secure에 로그 기록
```

**✅ 답안:**
```
1) /etc/pam.d/sshd
auth required pam_tally2.so deny=5 unlock_time=600

2) /etc/pam.d/system-auth
password requisite pam_pwquality.so minlen=10 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1

3) /etc/pam.d/system-auth
password required pam_pwhistory.so remember=5

4) /etc/pam.d/sshd
session required pam_unix.so
(기본적으로 /var/log/secure에 기록됨)
```

---

## 🔥 실기 시험 최빈출 PAM 개념

```
1위: PAM 4대 모듈 유형        ← 매년 출제! (auth, account, session, password)
2위: pam_tally2.so 계정 잠금  ← 자주 출제
3위: pam_pwquality.so 복잡성  ← 자주 출제
4위: PAM 설정 파일 위치       ← 가끔 출제 (/etc/pam.d/)
5위: 제어 플래그              ← 가끔 출제 (required, requisite)
```

**✅ 이 5개 개념만 정확히 외우면 PAM 문제 95% 커버!**
