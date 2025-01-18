# HTTP 이해하기

HTTP는 HyperText Transfer Protocol의 약자로, 서버와 클라이언트 간 정보를 주고받기 위한 통신 프로토콜임. 

HTTP는 **TCP/IP 기반**으로 작동하며, 웹 상에서 HTML, JSON과 같은 데이터 형식을 주고받는 데 사용됨. 

HTTP의 주요 특징은 다음과 같음:

- **Connectionless**: 
  - 서버와 클라이언트는 요청(Request)과 응답(Response)이 끝나면 연결을 끊음. 
  - 동시 접속자 수가 많아도 서버 리소스를 효율적으로 사용할 수 있음
- **Stateless**: 
  - 서버는 클라이언트의 상태를 유지하지 않음.
  - 로그인 상태와 같은 정보를 유지하려면 Cookie, Session, JWT 등의 추가 기술이 필요함

### HTTP 메시지 구조
HTTP 통신은 클라이언트가 서버로 요청(Request)을 보내고, 서버가 응답(Response)을 반환하는 방식으로 이루어짐. 

각 메시지의 구조는 아래와 같음:

1. **Request Message(요청)**
   - Start Line: 요청 메서드(GET, POST 등), 경로(URL Path), HTTP 버전
   - Headers: 추가 정보(Host, User-Agent 등)
   - Body: 데이터(payload, JSON 등, 선택적)

2. **Response Message(응답)**
   - Status Line: HTTP 버전, 상태 코드(Status Code), 상태 메시지(Status Message)
   - Headers: 서버 응답에 대한 정보(Content-Type 등)
   - Body: 반환할 데이터(HTML, JSON 등, 선택적)

### HTTP의 보안 문제와 HTTPS
HTTP는 데이터를 **평문(Text)**으로 전송하므로, 네트워크 상에서 데이터가 가로채기 당할 경우 민감한 정보가 유출될 수 있음. 

이를 해결하기 위해 HTTPS가 등장했으며, HTTPS는 HTTP에 **SSL/TLS 기반 암호화**를 추가하여 데이터를 안전하게 전송할 수 있도록 함.

---

## GET과 POST의 차이점

HTTP 요청 방식 중 가장 많이 사용되는 GET과 POST는 아래와 같은 차이점이 있음:

| 구분      | GET                                      | POST                                      |
|-----------|------------------------------------------|-------------------------------------------|
| **목적**  | 데이터를 요청(조회)                      | 데이터를 전송(등록, 수정)                 |
| **데이터 전달 방식** | URL 쿼리스트링으로 전달 (ex. `?key=value`) | HTTP Body에 포함                         |
| **데이터 크기 제한** | 제한 있음 (일반적으로 2KB ~ 8KB)         | 제한 없음 (서버/브라우저에 따라 달라짐)  |
| **보안성** | 낮음 (URL에 데이터 노출)                | 높음 (데이터가 Body에 숨겨짐)            |
| **캐싱**  | 가능                                     | 불가능                                   |
| **사용 예** | 검색, 데이터 조회                       | 로그인, 회원가입, 폼 제출 등             |

---

## HTTP Status Code (상태 코드)

HTTP 응답에서 상태 코드는 클라이언트가 요청한 작업에 대한 결과를 나타냄. 

주요 상태 코드는 다음과 같음:

### 1xx: 정보 응답
- **100 Continue**: 요청이 초기 단계에서 성공, 계속 진행 가능
- **101 Switching Protocols**: 프로토콜 변경 요청 성공

### 2xx: 성공
- **200 OK**: 요청 성공
- **201 Created**: 리소스 생성 성공 (ex. POST 요청)
- **204 No Content**: 요청 성공, 하지만 반환 데이터 없음

### 3xx: 리다이렉션
- **301 Moved Permanently**: 리소스가 영구적으로 이동함
- **302 Found**: 리소스가 일시적으로 이동함
- **304 Not Modified**: 캐시된 리소스를 사용하라는 의미

### 4xx: 클라이언트 에러
- **400 Bad Request**: 잘못된 요청
- **401 Unauthorized**: 인증 필요
- **403 Forbidden**: 권한 없음
- **404 Not Found**: 요청한 리소스가 존재하지 않음

### 5xx: 서버 에러
- **500 Internal Server Error**: 서버 내부 오류
- **502 Bad Gateway**: 게이트웨이/프록시 서버 오류
- **503 Service Unavailable**: 서버가 일시적으로 사용 불가능

---

## 요약
- HTTP는 서버-클라이언트 간 통신 프로토콜로, **Connectionless**와 **Stateless** 특징을 가짐
- GET과 POST는 목적, 데이터 전달 방식, 보안성 등에서 차이가 있음
- 상태 코드는 클라이언트가 요청한 작업에 대한 서버 응답 상태를 나타냄
- 보안 강화를 위해 HTTPS 사용을 권장함
