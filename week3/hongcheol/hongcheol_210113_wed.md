# HTTP 상태코드
클라이언트가 보낸 요청의 처리 상태를 알려주는 기능을 한다.
404에러, 500 에러 등등..
##   1xx - 요청이 수신되어 처리중
잘 사용하지않는다.
## 2xx - 정상 처리됨
1. 200 - ok(정상적으로 잘 처리해서 반환 한 경우)
2. 201 - created(post등으로 서버에서 자원을 생성한 경우)
3. 202 - accepted(요청이 접수되었으나 처리가 안된 경우 - 배치 처리 같은 곳에서 사용한다.)
4. 204 - no content(성공적으로 수행했는데, 응답 페이로드 본문에 보낼게 없는 경우)
## 3xx - 요청을 완료하려면 추가 행동이 필요
* 웹 브라우저는 3xx 응답 결과에 Location header가 있으면 그 위치로 자동으로 이동한다.
* 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동(301,308), 원래의 URL을 사 용하지않아서 검색 엔즌 등에서도 변경을 인지한다.
* 일시 리다이렉션 - 일시적인 변경(302,307,303), URI가 일시적으로 변경. 따라서 검색 엔진 등에서 URL을 변경하면 안된다. 302를 가장 많이 쓰는데, 303과 307을 권장함
* 특수 리다이렉션 - 결과 대신 캐시를 사용(300,304),
* *PRG* (Post/Redirect/Get) - Post했을 때, 이미 있으면 302 로 Location 주면서 일시적으로 리다이렉션하게해서 중복 주문과 같은 오류를 방지할 수 있다.

1. 300 - multiple choices(거의 안씀)
2. 301 - Moved Permanently
	리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음.
3. 308 - Permanent Redirect
	301과 기능은 같지만 리다이텍트시 요청 메서드와 본문을 유지한다.

4. 302 - Found
	리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있다. 301이랑 똑같다.
5. 303 - See Other
	리다이렉트시 요청 메서드가 GET으로 변경된다.
6. 307 - Temporary Redirect
	302와 기능이 같음. 리다이렉트시 요청 메서드와 본문이 유지된다. 요청 메서드 변경은 절대 불가능

7. 304 - Not Modified
	캐시를 목적으로 사용. 클라이언트에게 리소스가 수정되지않았음을 알려줌. 캐시에 있는게 써도되는건지 물어본거에 써도 된다고 알려줌. 바디메세지를 포함하면 안된다.(로컬캐시를 사용해야하기 때문이다)

## 4xx - 클라이언트 오류가 나서 서버가 요청을 처리할 수 없음.(잘못된 문법 등등)
오류의 원인이 클라이언트
1. 400 - Bad Request
	요청이 잘못된 것(요청 구문이나 API 스펙이 안맞은 경우) 백엔드에서 철저하게 다 막아주저야한다.
2. 401 - Unauthorized
	인증이 안된 경우. WWW-Authenticate 헤더와 함꼐 인증 방법을 설명
	인증(누구인지 확인) 인가(권한부여, ADMIN 권한처럼 특정 리소스에 접근할 수 있는 권한, 장고에서 어드민 페이지 같은 것을 의미)
3. 403 - Forbidden
	요청은 이해했으나, 승인은 거부한 경우. 인증은 됐지만, 인가가 안된 경우
4. 404 - Not Found
	요청 리소스가 서버에 없음. 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때.

## 5xx - 서버 문제, 서버가 정상 요청을 처리하지 못하는 경우.
* 서버 문제(디비 접근이 불가능하거나 NullPointerException이 터지는 경우 등등)
* 503 - Service Unavailable
	섭다하고 작업할 때 보여주는거. Retry-After헤더로 공지사항도 띄워줄 수 있음.
* 서버에서 문제가 터졌을 때, 500대 에러를 만들어야함. 데이터 숨기거나 할거는 다 400대로 던져야한다. 

* 즉, 비즈니스 로직 상의 예외케이스는 500대를 던지면 안된다. 
500대를 내는거는 쿼리에 문제가 있거나 디비에 문제가 있거나 널포인트가 뜬다거나하는 상황에서 사용한다.



인식할 수 없는 상태코드를 서버가 반환하면 클라이언트는 그 상위 개념에서 이해하면된다. ex) 299 -> 2xx 대로 생각.

# HTTP 헤더
## 일반 헤더
용도 - HTTP 전송에 필요한 모든 부가정보를 담고있다.
기존의 용어에서
Entity -> Representation
표현 = 표현 메타데이터 + 표현
메세지 바디(본문)을 통해서 표현 데이터를 전달한다.
베세지 본문 = payload
표현은 요청이나 응답에서 전달할 실제 데이터를 의미한다.
표현 헤더는 표현 데이터를 해석할 수 있는 정보(데이터유형, 길이, 압축 정보 등등)을 포함한다.
### 표현헤더
[image:F68A918E-71F0-4DC9-BDA8-027CEB406D99-15530-000011DB52376D45/스크린샷 2021-01-13 오전 10.33.55.png]
1. Content-Type: 표현 데이터의 형식
타입이 뭔지 알려준다. 미디어타입, 문자인코딩 등등
2. Content-Encoding : 표현 데이터의 압축방식
데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가. 뭘로 압축된건지 알려줘서 압축을 풀 수 있도록해줌. identity - 압축 안한다는 의미.
3. Content-Language: 표현 데이터의 자연언어(한국어, 영어 등등)
ko, en, en-US 등등.
4. Content-Length: 표현 데이터의 길이
바이트 단위. Transfer-Encoding 사용시에는 사용 불가능
전송과 응답에 둘 다 사용이 가능하다.

### 컨텐츠 네고시에이션(협상)
Accept: 클라이언트가 선호하는 미디어 타입 전달 
Accept-Charset: 클라이언트가 선호하는 문자 인코딩 
Accept-Encoding: 클라이언트가 선호하는 압축 인코딩 
Accept-Language: 클라이언트가 선호하는 자연 언어

우선순위
1. Quality Values(q) 값 사용하고 그 범위는0~1, 클수록 높은 우선순위를 가진다. 생략하면 1.
2. 구체적인 것이 우선순위를 가진다.  - 구체적인 것을 기준으로 미디어 타입을 맞춰준다.

협상헤더는 요청시에만 사용한다.
###  전송방식
#### 단순 전송
요청하면 응답을 주는데, Content-Length를 알고 그만큼 넘겨주는 것처럼. 요청 들어오면 그냥 바로 주는 것
#### 압축 전송
Content-Encoding을 추가해서 뭐로 압축된건지 알려줘야함.
#### 분할 전송
Transfer-Encoding - chunked를 사용
덩어리로 나눠서 보낼 수 있다.
#### 범위 전송
Range,Content-Range - 범위를 지정해서 전송받을 수 있다. 게임받을 때, 멈췄다가 다시 받으면 이어서 받아지는거 생각하면 편할듯하다.
### 일반정보
#### From
일반적으로 잘 사용하지않는다. 검색엔진에서 주로 사용하고, 요청할 때 사용한다.
#### Referer
현재 요청된 페이지의 이전 웹페이지 주소(블로그 유입 경로 생각) 어떤 경로로 들어왔는지를 기록, A->B로 이동하는 경우 B를 요청시 Referer:A를 포함해서 요청
#### User-agent
request 보낼 때, 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등등)을 담고있다. 특정 브라우저에서 장애가 발생하는지 확인할 수 있다. 통계정보를 뽑을 때도 유용하다.
#### Server
요청을 처리하는 ORIGIN 서버의 소프트웨어 정보.
프록시 서버나 캐시서버가 아닌, 나의 요청이 도착하는 마지막 서버(나의 표현 데이터를 만들어주는 서버)에대한 정보
#### Date
날짜 돌려줌. 응답에서 사용
### 특별한 정보(실제 애플리케이션에 영향을 줌)
#### Host
필수값! 하나의 서버가 여러 도메인을 처리해야할 때 사용한다.
하나의 IP 주소에 여러 도메인이 적용되어 있을 때 사용.
host가 없으면 요청이 들어왔을 때, 어디로 연결해야할지 모르는 경우가 있다.
그래서 host를 이용해서 서버에서 어디로 보내줄지 판단할 수 있게 해줘야한다.
#### Location
페이지 리다이렉션에 사용. 201에서 생성된 리소스 URI를 돌려줄 때나, 300대 에서 리다이렉션할 위치를 알려줄 때 사용됨.
#### Allow
허용 가능한 HTTP 메서드일 때, URL 경로는 있는데, 허용안된 메서드를 이용한 경우에 어떤게 가능한지 알려줄 때 사용
#### Retry - After
503에서 서비스가 언제까지 불능인지 알려줄 수 있음.
### 인증헤더
#### Authorization
클라이언트 인증 정보를 서버에 전달해줌. HTTP는 인증 메커니즘에 상관없이 헤더를 제공해준다.
#### WWW-Authenticate
리소스 접근시 필요한 인증 방법을 정의한다. 인증하려면 이런 정보들을 참고해서 제대로된 인증을 만들라는 응답을 할 때 쓴다.
### 쿠키
Set-Cookie와 Cookie를 사용한다.
http는 무상태 프로토콜이라서, 서버와 클라이언트는 서로의 상태를 저장하지않는다. 클라이언트와 서버의 연결이 끊어진 후에 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못하는 점을 해결해준다. 특히 요청과 링크에 사용자 정보를 포함하도록하면 보안에 취약하고, 개발도 어렵다. 그렇기 때문에, 정보를 쿠키에 담아서 웹브라우저 내부의 쿠키 저장소에 저장한다. 

Set-Cookie로 서버에서 사용자 정보를 담은 쿠키를 생성한다.
이렇게 생성해서 저장하면, 요청 보낼 때, Cookie: 를 이용해서 쿠키 저장소를 뒤져서 요청에 넣어서 유저 정보를 보내준다. 뭐로 보내든 지정한 서버에대해서는 쿠키의 데이터를 자동으로 뽑아서 보내준다.

로그인 세션 관리를 해줄 때 사용. 광고 정보를 트래킹할 때도 사용한다.
쿠키 정보는 항상 서버에 전송된다. 그로 인해 네트워크 트래픽을 추가 유발할 수 있다. 그래서 최소한의 정보만 사용해야한다.(ex. 세션 ID, 인증 토큰)
서버에 전송하기 싫을 때(클라이언트 로직에서만 사용하고싶을 때)는 웹 브라우저 내부의 웹 스토리지를 참고하면 된다.

쿠키는 2종류가 있다.
세션 쿠키 - 만료 날짜를 생략하면 브라우저 종료시에 만료된다.
영속 쿠키 - 지정한 날짜까지 살아있다가 지나면 만료된다.
expires와 max-age를 이용해서 생명주기를 관리할 수 있다.

쿠키 - 도메인.
명시.
명시한 문서 기준 도메인 + 서브 도메인 포함.
생략하면 현재 문서 기준 도메인만 포함(하위 도메인에서 접근 불가)

쿠키 - 경로.
일반적으로 path=/ 루트로 지정.
path=“여기 적힌 경로를 포함한 하위 경로 페이지만 쿠키에 접근”

쿠키 - 보안.
Secure - https인 경우에만 전송한다.
HttpOnly - 자바스크립트에서 접근 불가능하게 만들어서 XSS 공격을 방지한다.
SameSite - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키를 전송할 수 있게해줌. XSRF 공격을 방지한다.( 사용 시 브라우저에서 어느정도까지 지원하는지 확인해야한다.)

## 캐시와 조건부 요청
### 캐시가 없는 경우
데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다. 인터넷 네트워크는 매우 느리고 비싸다. 브라우저 로딩 속도가 느리다. 느린 사용자 경험 
### 캐시가 있는 경우
캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다. 비싼 네트워크 사용량을 줄일 수 있다. 브라우저 로딩 속도가 매우 빠르다. 빠른 사용자 경험 
한 번 들어갔던 브라우저 다시 들어가면 빨리 열리는 이유.

### 캐시시간초과. 
유효시간이 초과하면 서버를 통해서 데이터를 다시 조회하고 캐시를 갱신한다. 이때 다시 네트워크 다운이 발생하는데, 이런 불필요한 과정을 해결할 수 있는 메커니즘이 검증헤더와 조건부 요청이다.

### 검증 헤더
캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
#### Last-Modified
Last-Modified를 이용해서 최종 수정일을 추적한다. 검증할 때는 if-modified-since(If-Unmodified-Since는 반대로직)에 캐시에 있는 데이터 최종 수정일을 담아서 서버로 보내고 서버는 이 데이터를 Last-Modified를 보고 써도되는지 판단을해서 응답을 만들 때, 304 Not Modified를 보내고, 필요한 정보 헤더에 담아서 보낸다. 이때 HTTP Body를 안보낸다.(헤더만 보내서 보내야하는 크기가 줄어든다.-> 네트워크 부하도 준다.)
수정된 경우는 200 OK
단점은 1초 미만 단위로 캐시 조정이 불가능하다. 날짜 기반의 정해진 로직을 사용한다. 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우는 다른 데이터로 인식을해서 불필요한 다운로드가 발생할 수 있다. 
#### ETag
서버에서 별도의 캐시 로직을 관리하고 싶은 경우에 사용.
진짜 단순하게 ETag만 서버에 보내서 같으면 유지, 다르면 다시 받게한다.
캐시용 데이터에 임의의 고유한 버전 이름을 달아두고, 데이터가 변경되면 이름을 바꿔서 변경한다.(Hash를 다시 생성한다.) Hash의 특징을 이용해서 같은 데이터면 같은 값이 나오게했기때문에, 비교해서 ETag가 다른 경우에만 다운로드하도록한다.
If-None-Match 헤더에 캐시에 있는 ETag를 담아서 서버로 보낸다.
If-Match는 반대 로직
같으면 304 -> http body 안보냄. 
다르면 200 OK

캐시 제어 로직을 서버에서 완전히 관리하고 클라이언트는 단순하게 값을 서버에 제공하는 역할을 한다.


### 조건부 요청
캐시 만료 후에도 서버의 데이터가 안변한 경우에는 저장해둔 데이터를 써도 된다. 서버에 있는게 바뀐건지 확인하기 위해서 검증 헤더를 사용한다. 캐시에 저장된 데이터 재활용 가능한지 물어보는 요청을 포함해서 불필요한 다운로드를 줄이는 메커니즘이다.

### 캐시와 조건부요청 헤더
Cache-Control(얘로 다 해결 가능)
* max-age - 캐시 유효시간
* no-cache - 주의 데이터는 캐시해도 되지만, 항상 origin 서버에 검증하고 사용하라는 뜻
* no-store - 데이터에 민감한 정보가 있을 때 저장하면 안된다는 뜻. 메모리에서 사용하고 최대한 빨리 삭제하게함.
Pragma
* 캐시 컨트롤의 하위 호환으로 잘 사용안함.
Expires
* 만료일 지정. 만료일을 정확한 날짜로 지정해줌. max-age가 이 기능을 대신하고있음. max-age가 훨씬 유연하기때문.
## 프록시 캐시
웹 브라우저가 원서버가 아닌 프록시 캐시 서버를 확인하게 만든다. 물리적 거리가 가까워지면 빨라진다. 
* Cache-Control: public
응답이 public 캐시에 저장되어도 됨. 
* Cache-Control: private
응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값) 
아래 2개는 이런게 있다 정도만 알자
* Cache-Control: s-maxage
프록시 캐시에만 적용되는 max-age 
* Age: 60 (HTTP 헤더) 
오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초). 

## 캐시 무효화
이 데이터는 절대로 캐시되면 안돼! 하는 것에 붙여주기.
no-cache, no-store,must-revalidate
* Cache-Control: no-cache
데이터는 캐시해도 되지만, 항상 ORIGIN 서버에 검증하고 사용(이름에 주의!) 
ㄴPragma: no-cache .HTTP 1.0 하위 호환 
프록시 캐시서버에 갔는데, 아 이건 내가 처리하면 안되는거네 하면 원서버에 요청을 보낸다.
no-cache로 보냈는데, 프록시에서 ORIGIN 서버에 연결이 끊긴 경우에 에러를 출력하거나 이전의 데이터를 보여주거나 할 수 있다.
* Cache-Control: no-store
데이터에 민감한 정보가 있으므로 저장하면 안됨
(메모리에서 사용하고 최대한 빨리 삭제) 

* Cache-Control: must-revalidate

캐시 만료후 최초 조회시 원 서버에 검증해야함. 원 서버 접근 실패시 반드시 오류가 발생해야함
캐시 서버에 요청을 보냈는데, 순간적으로 네트워크 단절이 된 경우 ORIGIN 서버에 접근이 안되는 경우 항상 오류를 만들어 줘야한다는 뜻
-504(Gateway Timeout) must-revalidate는 캐시 유효 시간이라면 캐시를 사용함 