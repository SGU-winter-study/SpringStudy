## HTTP 헤더

#### 엔티티

HTTP 전송에 필요한 모든 부가정보(metadata)를 담고 있다.
* General 헤더: 메시지 전체에 적용
* Request 헤더: 요청 정보
* Response 헤더: 응답 정보
* Entity 헤더: 엔티티 바디 정보

엔티티 본문(entity body)은 요청/응답에 실제로 전달할 데이터를 담고 있다. 그리고 이 엔티티 본문을 담아서 보내기 위해 존재하는 것이 메시지 본문(message body)이다. 그리고 이 엔티티 본문을 해석하는데 필요한 정보(데이터 유형, 데이터 길이 등)를 담고 있는 것이 엔티티 헤더(entity header)이다.

#### 표현

엔티티(Entity)라는 말이 표현(Representation)으로 최근 들어 바뀜.
'표현'은 표현 메타데이터와 표현 데이터를 총칭하는 말이다. 이렇게 해서 바뀐 용어를 다시 정리하면,
* 페이로드(payload) = 메시지 본문
* 표현(representation) = 요청이나 응답에서 전달할 실제 데이터
* 표현 헤더(representation header) = 표현 데이터를 해석할 수 있는 정보
	- Content-Type: 표현 데이터 형식
	- Content-encoding: 표현 데이터의 압축 방식
	- Content-Langugae: 표현 데이터의 자연 언어
	- Content-Length: 표현 데이터의 길이

#### 협상
클라이언트가 원하는 표현을 서버에게 요청함. (서버가 그것을 못 할 수도 있지만, 최대한 노력함.) 요청 시에만 사용한다.
* Accept: 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset: 문자 인코딩
* Accept-Encoding: 압축 인코딩
* Accept-Language: 자연 언어

협상의 우선순위를 표현하기 위해 0~1 사이의 값을 갖는 Quality Values(q) 사용(값이 클수록 높은 우선순위를 가짐, 생략시 기본값 1). 만약 q값이 동일하다면, 구체적인 것이 더 높은 우선순위를 갖는다.

## HTTP 전송 방식
1. 단순 전송(Content-Length)
Content 길이를 알고 있을 때 해당 컨텐드를 한 번에 전송하는 것.
2. 압축 전송(Content-Encoding)
gzip과 같은 압축을 사용하여 전송하는 것. 무엇으로 압축했는지 Content-Encoding 필드를 헤드에 추가해야 한다.
3. 분할 전송(Transfer-Encoding)
컨텐트를 청크로 나누어 여러 번에 나누어 전송하는 것. 헤더에 Content-Length 정보를 넣으면 안 되고, Transfer-Encoding 정보를 넣어야 한다.
4. 범위 전송(Range, Content-Range)
특정 범위를 지정하여 전체가 아니라 해당 부분만 전송하는 것.


## HTTP 헤더의 정보
[일반 정보]
- From: 유저 에이전트의 이메일 정보
- Referer: 이전 페이지 주소(유입 경로 분석 가능)
- User-Agent: 클라이언트의 애플리케이션 정보
- Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
- Data: 메시지가 발생한 날짜와 시간

[특별한 정보]
- Host: 요청한 호스트의 정보(도메인). 요청에서 사용하는 필수 헤더값. 가상 호스팅에서 사용함
- Location: 페이지 redirection의 대상
- Allow: 허용되는 HTTP 메소드
- Retry After: 유저 에이전트가 다음 요청 시도까지 기다려야 하는 시간.

[인증 정보]
- Authorization: 클라이언트 인증 정보
- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의. 401 응답과 함께 사용.

[쿠키 정보]
HTTP는 무상태 프로토콜이기 때문에, 서버는 이전 요청을 알지 못한다. 이로 인해 발생하는 문제를 해결하기 위해 과거 요청에 대한 응답을 저장하는 쿠키(Cookie)를 사용한다.
- Set-Cookie: 서버에서 클라이언트로 응답시에 쿠키 전달
- Cookie: 서버로부터 받은 쿠키를 클라이언트가 저장하고, HTTP 요청시 서버에 전달
- Expires: 영속 쿠키의 쿠키 만료일(GMT 기준). 만료일이 생략되는 경우 브라우저 종료시까지만 유지됨(세션 쿠키).
- max-age: 쿠키의 생명주기 초단위 지정
- domain: 구체적인 도메인을 명시하는 경우, 해당 도메인과 서브 도메인에서 해당 쿠키에 접근이 가능. 생략하는 경우, 현재 문서에서(서브 도메인X)만 쿠키에 접근 가능
- path: 해당 경로를 포함한 하위 경로 페이지에서만 쿠키에 접근 가능.
- Secure: 적용시 https인 경우에만 전송(http는 X)
- HttpOnly: XSS 공격에 방지하여 JS에서는 접근이 불가하고 HTTP 전송에만 사용
- SameSite: XSRF 공격에 방지하여, 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키를 전송

