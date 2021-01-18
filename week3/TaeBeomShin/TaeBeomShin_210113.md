<h1>HTTP 웹 기본 지식 - 3일차(HTTP 헤더) </h1>

* HTTP 헤더 : HTTP 전송에 필요한 모든 부가정보(모든 메타정보), 표현 데이터를 해석할 수 있는 정보.
* HTTP 바디 : 메시지 본문(표현 데이터 - 요청이나 응답에서 전달할 실제 데이터)

## 표현(Representation)을 위한 헤더
: 요청이나 응답에서 전달할 실제 데이터, 이를 요약하기 위해 헤더는 어떤 정보를 담고 있는가?

- Content-Type: 표현 데이터의 형식 ex) text/html;charset=UTF-8, application/json
- Content-Encoding: 표현 데이터의 압축 방식 ex) gzip, deflate, identity(압축안함)
- Content-Language: 표현 데이터의 자연 언어 ex) ko, en, en-US
- Content-Length: 표현 데이터의 길이(바이트 단위) 

## 협상(콘텐츠 네고시에이션)
-> 클라이언트가 선호하는 "표현"을 요청, 서버에서 이에 따른 정보 전달(응답) // 없을 수도 있음.

1. 협상헤더
- Accept: 선호 미디어 타입
- Accept-Charset: 선호 문자 인코딩
- Accept-Encoding: 선호 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어


2. 협상과 우선순위

* 협상(요구사항)을 효율적, 효과적으로 반영하기 위해 Quality Value를 통해 값을 매긴다.

- 우선순위는 가중치가 높은 것이 우선된다.
- 우선순위는 구체적으로 작성한 것이 우선된다.
- 우선순위는 구체적인 기준에 미디어 타입을 맞춘다.

## 전송방식

- 단순 전송 : 한번에 요청하고 한번에 받는다.
- 압축 전송 : 여러번 요청하고 압축해서 받는다.(Content-Encoding)
- 분할 전송 : Transfer-Encoding, 말 그대로 나누어서 받는다.
- 범위 전송 : 필요한 범위만 지정에서 그 범위만 받는다.

## 일반 정보

- From : 유저 agent의 이메일 정보
- Referer : 이전 웹페이지 주소(유입경로 분석에 이용)
- User-Agent : 유저 에이전트 어플리케이션 정보(브라우저 정보)
- Server : 요청을 처리하는 orign server 정보(proxy가 아닌)
- Date : 응답 시에만 적용, 날짜 정보.

## 특별한 정보

- Host : 필수헤더, 요청한 호스트 정보(도메인)// 가상 호스팅시 오류를 막는다.
- Location : 페이지 리다이렉션 - 201, 3xx
- Allow : 허용가능한 http method. - 405
- Retry-After : 유저 agent가 다음 요청까지 기다려야 하는 시간. - 503
- Authorization : 클라이언트 인증 정보를 서버에 전달.
- WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의 - 401

## 쿠키

- Set-Cookie : 서버에서 클라이언트로 전달할 쿠키
- Cookie : 클라이언트가 서버에서 받은 쿠키 저장하고, Http 요청시 서버로 전달.
- 쿠키를 이용함으로써 효율성이 강화 된다. ex) 사용자 로그인 세션 관리, 광고 정보 트래킹
- 하지만 쿠키는 "추가 트래픽"이고 보안성이 좋지 않기 때문에 최소한, 민감하지 않은 정보만 보내야한다.

* 쿠키 정보

1. 생명주기 - expires : 만료일, 만료일이 되면 쿠키 삭제
           - max-age : 초단위로 만료일 저장(더 유연하므로 max-age를 보통 이용한다.)

2. 도메인 - 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함. ex) domain=example.org
         - 생략 : 현재 문서 기준 도메인만 적용

3. 경로(Path) - 경로를 포함한 "하위 경로 페이지"만 쿠키 접근 허용.

4. 보안 - Secure : https인 경우에만 쿠키 전송
        - HttpOnly : XSS 공격 방지. 자바스크립트에서 접근 불가능.
        - SameSite : XSRF 공격 방지. 요청도메인 == 설정 도메인 일때만 쿠키 전송.


## 캐시와 조건부 요청

* 캐시 : 유사하거나 동일한 정보를 중복해서 요청할 필요없이 클라이언트 사이드에 저장하는 방식.

* 캐시 + 조건부 요청

- 기존 캐시에서는 유효 시간이 초과하면 캐시를 갱신하는 형태로 사용.
- 조건부 요청에서는 Last-Modified 와 if-modified-since 를 비교하는 방법과
- 와 함께 If-None-Match 와 Etag를 이용하여 실제로 데이터가 변경 되었는지 확인하는 방법을 통해
- 캐시를 갱신한다.
- Last-Modified, if-modified-since : 304 + 헤더 메터 정보으로만 응답.
- Etag, If-None- Match : 캐시 제어로직을 서베에서 완전히 관리

## 프록시 캐시와 캐시 무효화

* 프록시 캐시 서버 : 원서버에서 클라이언트까지의 요청이 길어지는 경우, 중간에 프록시 서버를 두어
                    클라이언트가 빠르게 응답 받을수 있게하는 방법.

* Cache-Control
- public : 응답이 캐시에 저장되어도 된다.
- private : 응답은 무조건 private 캐시에만 저장되어야 한다.
- s-maxage : 프록시 캐시에만 적용된다.

* 캐시 무효화 

- 명령어: Cache-Control: no-cache, no-store, must-revalidate
          Pragma: no-cache