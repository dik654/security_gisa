# KISA Unix/Linux 취약점 점검 항목 - 실기 필수 암기

## 🔴 반드시 암기해야 하는 Top 10 취약점 항목

### U-01: root 계정 원격 접속 제한 ★★★
```
점검 내용:
root 계정의 원격 터미널 서비스 접속을 차단하고 있는지 점검

취약점 발생 조건:
- SSH, Telnet 등을 통한 root 직접 로그인 허용

보안 대책:
/etc/ssh/sshd_config 설정 변경
PermitRootLogin no

설정 적용:
systemctl restart sshd
```

**📝 기출 문제 1-1 (2023년 실기)**
```
SSH를 통한 root 계정의 원격 로그인을 차단하는
설정 파일의 경로와 설정 항목을 쓰시오.
```

**✅ 답안:**
```
파일 경로: /etc/ssh/sshd_config
설정 항목: PermitRootLogin no
```

---

### U-02: 패스워드 복잡성 설정 ★★★
```
점검 내용:
패스워드 최소 길이, 복잡성, 유효기간 등이 설정되어 있는지 점검

취약점 발생 조건:
- 패스워드 최소 길이 8자 미만
- 복잡성 미설정 (영문, 숫자, 특수문자 조합 없음)
- 최대 사용기간 90일 초과

보안 대책:
/etc/login.defs 설정
PASS_MAX_DAYS   90    # 최대 사용기간 90일
PASS_MIN_DAYS   1     # 최소 사용기간 1일
PASS_MIN_LEN    8     # 최소 길이 8자
PASS_WARN_AGE   7     # 경고 일수 7일

PAM 복잡성 설정 (/etc/pam.d/system-auth 또는 /etc/pam.d/common-password)
password requisite pam_pwquality.so retry=3 minlen=8 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1

설명:
- dcredit=-1 : 숫자 최소 1개
- ucredit=-1 : 대문자 최소 1개
- lcredit=-1 : 소문자 최소 1개
- ocredit=-1 : 특수문자 최소 1개
```

**📝 기출 문제 2-1 (2022년 실기)**
```
패스워드 최대 사용기간을 90일로 설정하고,
최소 길이를 8자로 설정하는 파일과 설정 항목을 쓰시오.
```

**✅ 답안:**
```
파일: /etc/login.defs

설정:
PASS_MAX_DAYS 90
PASS_MIN_LEN 8
```

---

### U-03: 계정 잠금 임계값 설정 ★★
```
점검 내용:
로그인 실패 시 계정 잠금 정책이 설정되어 있는지 점검

취약점 발생 조건:
- 로그인 연속 실패 임계값 미설정 (무제한 시도 가능)
- Brute Force 공격에 취약

보안 대책:
PAM 설정 (/etc/pam.d/system-auth 또는 /etc/pam.d/common-auth)

auth required pam_tally2.so deny=5 unlock_time=300

설명:
- deny=5 : 5회 실패 시 잠금
- unlock_time=300 : 300초(5분) 후 자동 해제

수동 해제:
pam_tally2 --user=사용자명 --reset
```

**📝 기출 문제 3-1**
```
로그인 5회 실패 시 계정을 잠그고, 5분 후 자동 해제되도록
PAM 설정을 작성하시오.
```

**✅ 답안:**
```
auth required pam_tally2.so deny=5 unlock_time=300
```

---

### U-04: 패스워드 파일 보호 ★★★
```
점검 내용:
/etc/passwd, /etc/shadow 파일의 소유자 및 권한이 적절한지 점검

취약점 발생 조건:
- /etc/shadow 파일을 일반 사용자가 읽을 수 있음
- /etc/passwd 권한이 644 초과

보안 대책:
올바른 권한 설정:

/etc/passwd
- 소유자: root
- 권한: 644 (rw-r--r--)
- 명령어: chmod 644 /etc/passwd

/etc/shadow
- 소유자: root
- 권한: 400 (r--------) 또는 000
- 명령어: chmod 400 /etc/shadow

/etc/group
- 소유자: root
- 권한: 644
- 명령어: chmod 644 /etc/group

/etc/gshadow
- 소유자: root
- 권한: 400 또는 000
- 명령어: chmod 400 /etc/gshadow
```

**📝 기출 문제 4-1 (2023년 실기)**
```
/etc/shadow 파일의 적절한 권한과 그 이유를 설명하시오.
```

**✅ 답안:**
```
권한: 400 (r--------)

이유:
/etc/shadow 파일에는 암호화된 패스워드가 저장되어 있으므로,
root만 읽을 수 있도록 설정하여 일반 사용자의 접근을 차단해야 함.
이를 통해 Rainbow Table 공격 등 암호 크래킹 시도를 방지할 수 있음.
```

---

### U-06: 파일 및 디렉토리 소유자 설정 ★★
```
점검 내용:
소유자가 존재하지 않는 파일 및 디렉토리가 있는지 점검

취약점 발생 조건:
- 소유자가 삭제된 후 남아있는 파일 (UID/GID만 표시)
- 공격자가 동일한 UID로 계정 생성 시 해당 파일 접근 가능

점검 방법:
# 소유자 없는 파일 찾기
find / -nouser -o -nogroup -print

보안 대책:
1. 소유자 없는 파일 확인 후 삭제 또는 적절한 소유자 지정
chown root:root [파일명]

2. 정기적인 점검 수행 (월 1회 이상)
```

**📝 기출 문제 6-1**
```
시스템 전체에서 소유자가 존재하지 않는 파일을
찾는 명령어를 쓰시오.
```

**✅ 답안:**
```
find / -nouser -print

또는

find / -nouser -o -nogroup -print
```

---

### U-07: /etc/passwd 파일 소유자 및 권한 설정 ★★
```
점검 내용:
/etc/passwd 파일의 소유자가 root이고 권한이 644 이하인지 점검

취약점 발생 조건:
- 일반 사용자가 쓰기 권한을 가진 경우
- 공격자가 UID 0인 계정 추가 가능

보안 대책:
chown root /etc/passwd
chmod 644 /etc/passwd

점검 방법:
ls -l /etc/passwd
-rw-r--r-- 1 root root ... /etc/passwd
```

---

### U-08: /etc/shadow 파일 소유자 및 권한 설정 ★★★
```
점검 내용:
/etc/shadow 파일의 소유자가 root이고 권한이 400 이하인지 점검

취약점 발생 조건:
- 일반 사용자가 읽기 가능한 경우
- 암호화된 패스워드 노출 (Rainbow Table 공격 위험)

보안 대책:
chown root /etc/shadow
chmod 400 /etc/shadow

점검 방법:
ls -l /etc/shadow
-r-------- 1 root root ... /etc/shadow
```

---

### U-43: root 홈, 패스 디렉토리 권한 및 패스 설정 ★★
```
점검 내용:
1. PATH 환경변수에 "." (현재 디렉토리)가 포함되어 있는지 점검
2. PATH에 포함된 디렉토리의 소유자가 root인지 점검
3. PATH에 포함된 디렉토리의 권한이 755 이하인지 점검

취약점 발생 조건:
- PATH에 "." 포함 시 트로이목마 실행 위험
- PATH 디렉토리에 일반 사용자 쓰기 권한 존재

보안 대책:
# PATH 확인
echo $PATH

# PATH에서 "." 제거
# ~/.bash_profile 또는 ~/.bashrc 수정
export PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# PATH 디렉토리 권한 확인
ls -ld /usr/bin /bin /usr/sbin /sbin

# 권한이 755 초과인 경우 수정
chmod 755 /usr/bin
```

**📝 기출 문제 43-1**
```
root 계정의 PATH 환경변수에서 보안상 위험한 설정과
그 이유를 설명하시오.
```

**✅ 답안:**
```
위험한 설정: PATH에 "." (현재 디렉토리) 포함

이유:
공격자가 악의적인 명령어를 현재 디렉토리에 생성한 경우,
root가 정상 명령어를 실행하려 할 때 악성 파일이 먼저 실행될 수 있음.
(트로이목마 공격)

예시:
공격자가 /tmp/ls 라는 악성 스크립트 생성
→ root가 /tmp에서 ls 명령어 실행
→ /bin/ls 대신 /tmp/ls가 먼저 실행됨

대책:
PATH에서 "." 제거하고 절대 경로만 포함
```

---

### U-44: 사용자, 시스템 시작파일 및 환경파일 소유자 및 권한 설정 ★
```
점검 내용:
사용자 환경변수 파일의 소유자와 권한이 적절한지 점검

대상 파일:
- .profile
- .kshrc
- .cshrc
- .bashrc
- .bash_profile
- .login
- .exrc
- .netrc

취약점 발생 조건:
- 환경파일을 다른 사용자가 수정 가능한 경우
- 공격자가 악의적인 명령어 삽입 가능

보안 대책:
# 소유자: 해당 사용자
# 권한: 640 이하 (rw-r-----)

chmod 640 ~/.bashrc
chmod 640 ~/.bash_profile
```

---

### U-45: 세션 타임아웃 설정 ★★
```
점검 내용:
일정 시간 동안 입력이 없을 경우 자동 로그아웃 설정이 되어 있는지 점검

취약점 발생 조건:
- 세션 타임아웃 미설정
- 사용자 이석 시 타인이 시스템 접근 가능

보안 대책:
/etc/profile 또는 개별 사용자 ~/.bash_profile에 추가

# Bash Shell
export TMOUT=600    # 600초(10분) 후 자동 로그아웃

# C Shell
set autologout=10   # 10분 후 자동 로그아웃

# Korn Shell
export TIMEOUT=600

설정 적용:
source /etc/profile

권장 시간:
- 일반 사용자: 600초 (10분)
- 관리자: 300초 (5분)
```

**📝 기출 문제 45-1 (2023년 실기)**
```
Bash Shell에서 600초(10분) 동안 입력이 없으면
자동 로그아웃되도록 설정하는 환경변수와 값을 쓰시오.
```

**✅ 답안:**
```
TMOUT=600
또는
export TMOUT=600
```

---

### U-62: 로그의 정기적 검토 및 보고 ★★
```
점검 내용:
1. 접속 기록 등 각종 로그 기록의 정기적 검토, 분석, 리포트 작성 및 보고 여부 점검
2. 로그 보관 정책 수립 여부

점검 대상 로그:
- /var/log/messages (시스템 전반 로그)
- /var/log/secure (인증 관련 로그)
- /var/log/wtmp (로그인 성공 기록)
- /var/log/btmp (로그인 실패 기록)
- /var/log/lastlog (마지막 로그인 정보)

취약점 발생 조건:
- 로그 검토 정책 미수립
- 주기적인 로그 분석 미실시

보안 대책:
1. 로그 검토 주기 설정 (일/주/월)
   - 보안 로그: 일일 검토
   - 시스템 로그: 주간 검토

2. 로그 보관 정책
   - 온라인 보관: 최소 1개월
   - 오프라인 백업: 최소 1년 (개인정보 포함 시)

3. 로그 점검 항목
   - 비정상 로그인 시도 (lastb)
   - su, sudo 사용 내역 (/var/log/secure)
   - 시스템 오류 및 경고
   - 권한 변경 이력
```

**📝 기출 문제 62-1**
```
로그인 실패 기록을 확인하는 명령어와
해당 로그가 저장되는 파일의 경로를 쓰시오.
```

**✅ 답안:**
```
명령어: lastb
파일 경로: /var/log/btmp
```

---

## 📝 종합 문제: KISA 취약점 점검 시나리오

### 종합 문제 1 (2022년 실기 유사)
```
다음 요구사항에 따라 Linux 시스템의 보안을 강화하시오.

1) root 계정의 원격 로그인을 차단
2) 일반 사용자 패스워드 최대 사용기간 90일 설정
3) 로그인 5회 실패 시 계정 잠금, 10분 후 자동 해제
4) Bash Shell 사용자의 세션 타임아웃 10분 설정

각 요구사항에 대해 설정 파일 경로와 설정 내용을 쓰시오.
```

**✅ 답안:**
```
1) root 원격 로그인 차단
   파일: /etc/ssh/sshd_config
   설정: PermitRootLogin no
   적용: systemctl restart sshd

2) 패스워드 최대 사용기간 90일
   파일: /etc/login.defs
   설정: PASS_MAX_DAYS 90

3) 계정 잠금 정책
   파일: /etc/pam.d/system-auth
   설정: auth required pam_tally2.so deny=5 unlock_time=600

4) 세션 타임아웃 10분
   파일: /etc/profile 또는 ~/.bash_profile
   설정: export TMOUT=600
```

---

### 종합 문제 2 (점검 및 조치)
```
다음은 Linux 시스템 점검 결과이다. 취약점과 조치 방법을 쓰시오.

$ ls -l /etc/shadow
-rw-r--r-- 1 root root 1234 Nov 5 14:30 /etc/shadow

$ echo $PATH
.:/usr/local/bin:/usr/bin:/bin

$ grep PASS_MAX_DAYS /etc/login.defs
PASS_MAX_DAYS 99999
```

**✅ 답안:**
```
1. /etc/shadow 권한 취약점
   현재: 644 (rw-r--r--)
   문제: 일반 사용자가 읽기 가능, 암호화된 패스워드 노출
   조치: chmod 400 /etc/shadow

2. PATH 설정 취약점
   현재: PATH 맨 앞에 "." (현재 디렉토리) 포함
   문제: 트로이목마 공격 위험
   조치: ~/.bash_profile 수정
         export PATH=/usr/local/bin:/usr/bin:/bin
         (맨 앞의 "." 제거)

3. 패스워드 유효기간 취약점
   현재: PASS_MAX_DAYS 99999 (약 273년)
   문제: 패스워드 변경 주기 없음
   조치: /etc/login.defs 수정
         PASS_MAX_DAYS 90
```

---

## 🎯 KISA 취약점 점검 핵심 암기 카드

### 카드 1: 파일 권한 (최빈출!)
```
/etc/passwd   → 644 (rw-r--r--)
/etc/shadow   → 400 (r--------)
/etc/group    → 644
/etc/gshadow  → 400
```

### 카드 2: SSH 보안 설정
```
파일: /etc/ssh/sshd_config

PermitRootLogin no              # root 원격 로그인 차단
PasswordAuthentication no       # 패스워드 인증 비활성화
Protocol 2                      # SSH v2만 사용
```

### 카드 3: 패스워드 정책
```
파일: /etc/login.defs

PASS_MAX_DAYS 90    # 최대 90일
PASS_MIN_DAYS 1     # 최소 1일
PASS_MIN_LEN 8      # 최소 8자
PASS_WARN_AGE 7     # 7일 전 경고
```

### 카드 4: 계정 잠금
```
파일: /etc/pam.d/system-auth

auth required pam_tally2.so deny=5 unlock_time=300

deny=5           → 5회 실패
unlock_time=300  → 300초(5분) 후 해제
```

### 카드 5: 세션 타임아웃
```
Bash: TMOUT=600 (10분)
C Shell: set autologout=10
Korn Shell: TIMEOUT=600

설정 위치: /etc/profile 또는 ~/.bash_profile
```

---

## 📊 그림으로 이해하기

### KISA 점검 프로세스
```
[1단계: 점검 준비]
   └─ 점검 대상 시스템 확인
   └─ 점검 도구 준비
   └─ 점검 체크리스트 확인

[2단계: 취약점 점검]
   ├─ 계정 관리 (U-01~U-08)
   │   └─ root 원격접속, 패스워드 정책, 파일 권한
   ├─ 파일 및 디렉토리 관리 (U-43~U-45)
   │   └─ PATH 설정, 환경파일, 세션 타임아웃
   └─ 로그 관리 (U-62)
       └─ 로그 점검, 보관 정책

[3단계: 결과 분석]
   └─ 양호 / 취약 판정
   └─ 위험도 평가 (상/중/하)

[4단계: 조치]
   └─ 취약점 조치 계획 수립
   └─ 설정 변경 및 적용
   └─ 재점검

[5단계: 보고]
   └─ 점검 결과 리포트 작성
   └─ 조치 내역 문서화
```

### 파일 권한 판정 기준
```
/etc/shadow 점검 예시:

-rw-r--r-- 1 root root ... /etc/shadow  → 취약 (X)
   ↑  ↑  ↑
   │  │  └─ 기타 사용자 읽기 가능 (위험!)
   │  └─ 그룹 읽기 가능 (위험!)
   └─ 소유자 읽기/쓰기 가능

-r-------- 1 root root ... /etc/shadow  → 양호 (O)
   ↑
   └─ root만 읽기 가능 (안전)
```

---

## 💡 실기 답안 작성 팁

### Tip 1: 파일 경로는 절대 경로로
```
✅ /etc/ssh/sshd_config
❌ sshd_config
❌ /etc/sshd_config
```

### Tip 2: 권한은 숫자로 명확히
```
✅ 644
✅ 400
❌ rw-r--r-- (숫자가 더 명확)
```

### Tip 3: 설정 적용까지 작성
```
✅ systemctl restart sshd
✅ service sshd restart
✅ source /etc/profile
```

### Tip 4: U-번호와 내용 매칭
```
U-01: root 원격 접속 제한
U-02: 패스워드 복잡성
U-04: 패스워드 파일 보호
U-45: 세션 타임아웃
U-62: 로그 정기 검토
```

---

**✅ KISA 취약점 점검 Top 10만 정확히 외우면 실기 Unix/Linux 보안 문제 80% 커버!**
