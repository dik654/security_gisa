# Linux 시스템 추가 기출 - 필수 암기

## 🔴 ls 명령어 "+" 의미 ★★★

### 문제 (2024년 기출)
```
ls 명령어를 실행한 결과 "+"가 나오면 그 의미는 무엇인가?
```

### ✅ 정답
```
ACL (Access Control List) 설정되어 있음을 의미

예시:
$ ls -l
-rw-rw-r--+ 1 user group 1234 Nov 5 14:30 file.txt
           ↑
         ACL 존재

"+": 기본 권한(rwx) 외에 추가 ACL 설정됨
```

### ACL이란?
```
기본 권한: 소유자, 그룹, 기타 (3가지만)
ACL: 특정 사용자/그룹에게 세밀한 권한 부여 가능

예시:
파일 소유자: user01
기본 그룹: group01
ACL 추가: user02에게 읽기 권한 부여
         user03에게 쓰기 권한 부여
```

### ACL 명령어
```
1. ACL 확인
getfacl file.txt

출력 예시:
# file: file.txt
# owner: user01
# group: group01
user::rw-
user:user02:r--    # user02에게 읽기 권한
group::rw-
mask::rw-
other::r--

2. ACL 설정
setfacl -m u:user02:r file.txt   # user02에게 읽기 권한
setfacl -m g:group02:rw file.txt # group02에게 읽기/쓰기

3. ACL 제거
setfacl -x u:user02 file.txt     # user02 ACL 제거
setfacl -b file.txt               # 모든 ACL 제거
```

---

## 🔴 Linux 파일 시스템 ★★

### 문제 (2024년 기출)
```
리눅스 파일시스템의 이름은?
```

### ✅ 정답
```
주요 Linux 파일시스템:

1. ext4 (Fourth Extended Filesystem) ★★★
   - 가장 많이 사용
   - ext2, ext3의 후속
   - 최대 파일 크기: 16TB
   - 저널링 지원 (시스템 장애 복구)

2. ext3 (Third Extended Filesystem)
   - ext2 + 저널링
   - 안정성 높음

3. ext2 (Second Extended Filesystem)
   - 구형, 저널링 미지원

4. XFS
   - 대용량 파일 시스템
   - RHEL/CentOS 기본 (7 이상)

5. Btrfs (B-tree Filesystem)
   - 스냅샷, 압축, RAID 지원
   - 차세대 파일시스템

6. NTFS
   - Windows 파일시스템
   - Linux에서도 마운트 가능 (ntfs-3g)

실기에서는 "ext4" 또는 "ext3" 답안이 가장 안전
```

---

## 🔴 좀비 프로세스 확인 방법 ★★★

### 문제 (2024년 기출)
```
좀비 프로세스 확인방법?
```

### ✅ 정답
```
1. ps 명령어로 확인 ★★★
ps aux | grep defunct
또는
ps aux | grep Z

출력 예시:
user  1234  0.0  0.0      0     0 ?   Z  14:30  0:00 [process] <defunct>
                                  ↑
                                STATE: Z (Zombie)

2. top 명령어
top
→ 상단에 "zombie" 개수 표시
Tasks: 150 total, 1 running, 148 sleeping, 0 stopped, 1 zombie

3. 상세 확인
ps -eo pid,ppid,stat,comm | grep Z

출력:
PID  PPID  STAT  COMMAND
1234 1000  Z+    process <defunct>
          ↑
        Z = Zombie
```

### 좀비 프로세스란?
```
정의:
프로세스가 종료되었지만 부모 프로세스가 wait()를 호출하지 않아
프로세스 테이블에 남아있는 상태

특징:
- CPU, 메모리는 사용 안 함
- PID와 프로세스 테이블 항목만 차지
- 많이 쌓이면 PID 고갈 가능

생성 원인:
부모 프로세스가 자식 프로세스 종료를 처리하지 않음

해결 방법:
1. 부모 프로세스 종료
   kill -9 <PPID>
   → 좀비가 init(PID 1)에 입양되어 정리됨

2. 부모 프로세스 수정
   wait() 또는 waitpid() 호출하도록 코드 수정
```

---

## 🔴 RDP 공격 시 사용하는 명령어 ★★

### 문제 (2024년 기출)
```
리눅스에서 RDP 공격 시 사용하는 명령어는?
```

### ✅ 정답
```
Nmap ★★★

RDP 포트 스캔:
nmap -p 3389 192.168.1.0/24

RDP 무차별 대입 공격 (Brute Force):
nmap -p 3389 --script rdp-brute --script-args userdb=users.txt,passdb=pass.txt 192.168.1.100

기타 RDP 공격 도구:
1. Hydra
   hydra -l administrator -P passwords.txt rdp://192.168.1.100

2. Metasploit
   use auxiliary/scanner/rdp/rdp_scanner
   use auxiliary/scanner/rdp/ms12_020_check

3. rdesktop (정상 접속 도구, 하지만 공격에 사용 가능)
   rdesktop 192.168.1.100
```

---

## 🔴 Wi-Fi 버퍼 오버플로우 취약점 ★

### 문제 (2024년 기출)
```
Wi-Fi 버퍼 오버플로우 취약점?
```

### ✅ 정답
```
KRACK (Key Reinstallation Attack) 또는
FragAttacks (Fragmentation and Aggregation Attacks)

주요 Wi-Fi 취약점:

1. KRACK (2017년)
   - WPA2 프로토콜 취약점
   - 4-way handshake 재설치 공격
   - 암호화 키 재사용으로 패킷 복호화

2. FragAttacks (2021년)
   - Wi-Fi 프레임 분할/집합 처리 취약점
   - 버퍼 오버플로우 포함
   - 악성 코드 실행 가능

3. Wi-Fi Protected Setup (WPS) PIN Brute Force
   - WPS PIN 8자리 무차별 대입
   - 도구: Reaver, Bully

방어:
- 펌웨어 업데이트
- WPA3 사용
- WPS 비활성화
```

---

## 🔴 Windows 자동 로그인 ★

### 문제 (2024년 기출)
```
윈도우 자동 로그인?
```

### ✅ 정답
```
Autologon ★★★

설정 방법:

1. 레지스트리 방법
   경로: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon

   설정값:
   AutoAdminLogon = 1
   DefaultUserName = 사용자명
   DefaultPassword = 패스워드

2. Sysinternals Autologon 도구
   https://docs.microsoft.com/en-us/sysinternals/downloads/autologon
   GUI로 간편 설정

3. netplwiz 명령어
   netplwiz (또는 control userpasswords2)
   → "사용자 이름과 암호를 입력해야 이 컴퓨터를 사용할 수 있음" 체크 해제

보안 주의:
- 패스워드가 레지스트리에 평문 저장됨 (LSA Secret 암호화)
- 물리적 보안이 중요한 환경에서는 사용 금지
```

---

## 🎯 Linux 추가 핵심 암기 카드

### 카드 1: ls + 의미
```
"+": ACL 설정됨
확인: getfacl 파일명
설정: setfacl -m u:user:r 파일명
```

### 카드 2: 파일시스템
```
ext4 (가장 많이 사용) ★
ext3
XFS (RHEL 7+)
```

### 카드 3: 좀비 프로세스
```
확인: ps aux | grep defunct
또는: ps aux | grep Z
해결: 부모 프로세스 종료
```

### 카드 4: RDP 공격 도구
```
nmap -p 3389 --script rdp-brute
hydra rdp://IP
```

---

## 💡 실기 답안 작성 팁

### Tip 1: ls +의 의미
```
✅ ACL (Access Control List) 설정됨

❌ 실행 권한 (틀림)
❌ 링크 파일 (틀림)
```

### Tip 2: 파일시스템
```
✅ ext4 (안전한 답)
✅ ext3 (정답)
✅ XFS (RHEL 환경)

❌ NTFS (Windows 파일시스템)
```

### Tip 3: 좀비 프로세스 확인
```
✅ ps aux | grep defunct
✅ ps aux | grep Z
✅ top (상단 zombie 확인)

❌ kill -9 (확인이 아니라 제거 방법)
```

---

## 🔥 실기 시험 최빈출 Linux 개념

```
1위: ls + 의미 (ACL)       ← 2024년 출제!
2위: 좀비 프로세스 확인    ← 2024년 출제!
3위: 파일시스템 (ext4)     ← 2024년 출제!
4위: RDP 공격 도구 (nmap)  ← 2024년 출제!
5위: Autologon             ← 2024년 출제!
```

**✅ ACL, 좀비 프로세스는 세부 개념! 정확히 암기 필수!**
