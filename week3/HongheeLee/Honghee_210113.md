## HTTP 상태코드

클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능. 크게 5가지로 나눠짐.

- ( 1xx (Informational): 요청이 수신되어 처리중)
-  2xx (Successful): 요청 정상 처리
-  3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
-  4xx (Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
-  5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함

#### 2xx - 성공 

클라이언트의 요청을 성공적으로 처리. 

- 200 OK : 요청 성공 ex) GET 요청에 성공적으로 응답할 때

- 201 Created : 요청 성공해서 새로운 리소스 생성 ex) POST 요청에 새로운 리소스 성공적으로 생성할 때

- 202 Accepted : 요청이 접수되었으나 처리가 완료되지 않았음. ex) 배치 처리

- 204 No Content : 서버가 요청을 성공적으로 수행했지만 응답 페이로드 본문에 보낼 데이터 X. ex) 웹 문서 편집기 save 버튼 -> 결과로 아무 내용 없어도 되는. 

#### 3xx (Redirection)

요청을 완료하기 위해 유저 에이전트의 추가 조치 필요(주로 웹 브라우저)

**리다이렉션**

​	웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)

​	종류 

 - 영구 리다이렉션(301, 308) - 특정 리소스의 URI가 영구적으로 이동. 

   301(Moved Permanently) : 리다이렉트시 요청 메서드가 GET으로 변하고, 본문 제거될 수도

   308(Permanent Redirect) : 301과 기능 같은데, 요청 메서드와 본문 유지. 

   BUT 실무에선 주로 일시 리다이렉션 사용.

 - 일시 리다이렉션 - 일시적인 변경

   **302 Found** : 리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될 수도. 실무에서 많이 사용

   307 Temporary Redirect : 302와 기능 같음. 리다이렉트시 요청 메서드와 본문 유지.

   303 See Other : 302와 기능 같음. 리다이렉트시 요청 메서드가 GET으로 변경.

   예시 : PRG(Post/Redirect/Get) 이후 리다이렉트. POST로 주문 후에 새로 고침으로 인한 중복 주문 방지. 

   자동 리다이렉션시에 GET으로 변해도 되면 그냥 302를 사용해도 문제 없다. 

 - 특수 리다이렉션 - 결과 대신 캐시를 사용

   304 Not Modified : 캐시를 목적으로 사용. 클라이언트에게 리소스가 수정되지 않았음을 알려줌. 클라이언트는 저장된 캐시를 재사용. (캐시로 리다이렉트)

#### 4xx (Client Error)

오류의 원인이 클라이언트에 있음(잘못된 문법 등). 요청 자체의 문제로 **똑같은 재시도도 실패함. **

- 400 Bad Request : 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음. ex) 요청 파라미터, API 스펙 맞지 않을 때

- 401 Unauthorized : 클라이언트가 해당 리소스에 대한 인증이 필요함.  ex) 로그인

- 403 Forbidden : 서버가 요청을 이해했지만 승인을 거부함. 인증 자격 증명은 있지만 접근 권한이 불충분한 경우

- 404 Not Found : 요청 리소스를 찾을 수 없음. 요청 리소스가 서버에 없거나 클라이언트가 권한 부족한 리소스에 접근해서 숨기고 싶을 때

#### 5XX (Server Error)\

서버 문제로 오류 발생. **재시도 하면 성공할 수도** 

- 500 Internal Server Error : 서버 문제로 오류 발생. 애매하면 이거.

- 503 Service Unavailable : 서비스 이용 불가. 과부하 또는 예정된 작업으로 잠시 요청 처리 X

## HTTP 헤더 1 - 일반 헤더

header-field = field-name ":" OWS field-value OWS

용도 : HTTP 전송에 필요한 모든 부가 정보 ex) 메시지 바디의 내용, 크기, 압축 등등

RFC2616(과거) : 분류 -> General 헤더, Request 헤더, Response 헤더, Entity 헤더. 

RFC723x(현재) : 엔티티->표현 = 표현 메타데이터 + 표현 데이터(메시지 본문을 통해 전달)

표현은 요청이나 응답에서 전달할 실제 데이터, 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공

#### 표현

- Content-Type : 표현 데이터의 형식 설명. 미디어 타입, 문자 인코딩. ex ) text/html; charset=utf-8 | application/json

- Content-Encoding : 표현 데이터 인코딩. 표현 데이터를 압축하기 위해 사용. 데이터 읽는 쪽에서 인코딩 헤더 정보로 압축 해제. ex ) gzip, deflate 

- Content-Language : 표현 데이터의 자연 언어를 표현. ex) ko, en, en-US

- Content-Length : 표현 데이터의 길이. 바이트 단위.

#### 협상(콘텐츠 네고시에이션)

클라이언트가 선호하는 표현 요청. 협상 헤더는 요청시에만 사용한다. 

- Accept: 클라이언트가 선호하는 미디어 타입 전달 
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

협상과 우선순위 

- Quality Values(q) 값 사용하고 0~1 클수록 높은 우선순위이다. 생략하면 1.

- 구체적인 것이 우선한다. 

- 구체적인 것을 기준으로 미디어 타입을 맞춘다.

#### 전송방식

- 단순 전송 : Content-Length사용. 길이를 알 때, 한번에 요청하고 한 번에 쭉 받는다. 

- 압축 전송 : Content-Encoding 사용. gzip 등을 사용해 압축해서 전송. Content-Encoding 줘서 클라이언트가  해제할 수 있도록.

- 분할 전송 : Transfer-Encoding 사용. 용량이 큰 걸 전송할 때 한번에 보내지 않고 정해진 크기대로 나눠서 전송. Content-Length 넣으면 안된다.

- 범위 전송 : Range(요청), Content-Range(응답) 사용. 

#### 일반 정보

- From : 유저 에이전트의 이메일 정보, 검색엔진 등에서 사용. 요청에서 사용.

- Referer : 현재 요청된 페이지의 이전 웹 페이지 주소. 유입 경로 분석 가능. 요청에서 사용

- User-Agent : 유저 에이전트(클라이언트) 애플리케이션 정보. 어떤 종류의 브라우저에서 장애가 발생하는지 파악. 요청에서 사용.

- Server : 요청을 처리하는 ORIGIN 서버(진짜 요청이 닿아서 표현 데이터를 만들어주는 실제 서버)의 소프트웨어 정보. 응답에서 사용.

- Date  : 메시지가 발생한 날짜와 시간. 응답에서 사용.

#### 특별한 정보

- Host : 요청한 호스트 정보(도메인). 요청에서 사용하는 필수 정보. 하나의 서버가 여러 도메인을 처리해야할 때 도메인을 구분할 수 있게 해준다. 

- Location : 페이지 리다이렉션. 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동이동한다. 

- (Allow : 허용가능한 HTTP 메서드. 405에서 응답에 포함해야 함.)

- Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야하는 시간. 503에서 서비스가 언제까지 불능인지.

#### 인증

- Authorization: 클라이언트 인증 정보를 서버에 전달
- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의. 401 Unauthorized 응답과 함께 사용. 필요한 인증 방법 알려줌. 

#### 쿠키

2가지 헤더 사용.

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

HTTP는 무상태 프로토콜이다. 클라이언트가 다시 요청하면 서버는 이전 요청 기억 X -> 쿠키로 해결

쿠키 정보는 항상 서버에 전송되기 때문에 최소한의 정보만 사용. 웹 브라우저 내부에 저장하고 싶으면 웹 스토리지에.  

- 생명주기 expires, max-age : 로 삭제 시점을 지정한다. 

- 도메인 domain: 으로 명시하면 기준 도메인 + 서브 도메인 쿠키 접근. 생략하면 기준 도메인에만 쿠키 접근

- 경로 Path : 이 경로를 포함한 하위 경로 페이지만 쿠키 접근

- 보안: Secure(https에만), HttpOnly(JS 접근 불가, HTTP에만), SameSite

## HTTP 헤더2 - 캐시와 조건부 요청

#### 캐시 기본 동작

캐시가 없으면 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운받아야 함. 

캐시 적용하면 캐시 가능 시간 동안 네트워크를 사용하지 않아도 된다. 

과정 : 요청->캐시 유효시간 검증->캐시에서 조회

캐시 시간 초과하면 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다.

#### 검증 헤더와 조건부 요청

캐시 유효 시간이 초과해서 서버에 다시 요청할 때 서버에서 기존 데이터를 변경하지 않았으면 저장해두었던 캐시 재사용할 수 있다. 클라이언트 데이터와 서버 데이터가 같다는 사실 확인되면! 이를 위해 **검증헤더** 사용. Last-Modified

캐시 유효 시간이 초과해도 서버의 데이터 갱신되지 않으면  304 Not Modified + 헤더 메타 정보만 응답(HTTP Body가 없음) -> 캐시 데이터 재사용, 캐시 메타정보 갱신. 

검증 헤더 : 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터. ex) Last-Modified, ETag

조건부 요청 헤더 : 검증 헤더로 조건에 따른 분기. 조건 만족하면(변경시) 200 OK, 조건 만족하지 않으면(미변경시) 304 Not Modified ex) If-Modified-Since, If-Node-Match

Last-Modified : 날짜 기반의 로직 | ETag : 캐시용 데이터에 임의한 고유한 버전 이름 달아 관리.

#### 캐시와 조건부 요청 헤더

- Cache-Control : 캐시 지시어
  - max-age : 캐시 유효 시간, 초 단위  
  - no-cache : 데이터는 캐시해도 되지만 항상 원서버에 검증하고 사용
  - no-store : 데이터에 민감한 정보 있으므로 저장X

- (Pragma : 캐시 제어, no-cache, 1.0 하위 호환)

- (Expires : 캐시 만료일 정확한 날짜로 지정. 하위 호환)

- 검증 헤더 : ETag, Last-Modified

- 조건부 요청 헤더 : If-Match, If-None-Match, If-Modified-Since, If-Unmodified-Since

#### 프록시 헤더

private 캐시는 개별 웹 브라우저나 로컬 pc에 저장된다. public 캐시는 프록시 캐시 서버에 저장된다. 

Cache-Control: public ->응답이 public 캐시에 저장되어도 됨

Cache-Control: private -> 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)

#### 캐시 무효화

확실한 캐시 무효화 응답을 위해 

Cache-Control: no-cache, no-store, must-revalidate | Pragma: no-cache 사용한다.

Cache-Control: must-revalidate -> 캐시 만료후 최초 조회시 원 서버에 검증해야함, 접근 실패시 반드시 오류 발생(504). no-caches는 그래도 캐시 데이터 반환할 수도 있음(차이점).
