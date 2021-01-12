<h1>HTTP 웹 기본 지식 - 1일차(인터넷 네트워크 ~ HTTP 기본) </h1>

## 인터넷 네트워크 - IP, TCP/UDP, PORT, DNS

* 1. IP(Internet Protocol)
- 지정한 IP주소(목적지)에 데이터 전달
- "패킷" 단위로 데이터 전달 
- IP 패킷 : 출발지 IP, 목적지 IP

* IP프로토콜 한계
- 비연결성 : 패킷 대상이 없거나, 서비스 중이지 않더라도 패킷전송(상대 정보 모름)
- 비신뢰성 : 중간에 유실되거나, 순서대로 오지않을 수 있음(중간 경로 모름).
- 프로그램 구분 : IP주소아래에 어플리케이션이 여러개 있을때 구분 불가능.

* 2. TCP
- TCP : IP 프로토콜의 한계를 해결 -> TCP 정보생성, 전송제어 목적.
- TCP 전송 정보 : 출발/도착 PORT, 전송 제어, 순서, 검증정보.

* TCP 특징
- 연결지향 - 3 way handshaking(가상연결) : syn, syn+ack, ack
- 데이터 전달 보증 <-> ack
- 순서보장 : 순서 다르면 순서 바뀐 곳 부터 재요청.
- 전송제어를 통해 "신뢰"할 수 있는 프로토콜

* 3. UDP
- UDP : IP + PORT + checksum.
- 보기처럼 단순하므로 빠르지만, 신뢰성은 확보되지 않음.
- 최근 http spec에서는 udp가 tcp를 대체할 것으로 예상.

* 4. PORT
- 같은 IP내에서 프로세스 구분
- 각 포트별로 역할(?) 할당.
- FTP:20,21 , TELNET:23, HTTP:80, HTTPS:443

* 5. DNS
- IP가 기억하기 어렵고, 변경될 시 생기는 문제를 해결하기 위해 등장.
- 도메인명에 IP주소를 기억하고 도메인 주소를 통해 서칭.
- IP주소가 바뀌어도 바뀐 IP를 도메인에 매칭하기만 하면 됨.

## URI/HTTP 기본.

* 1. URI
- URI : Uniform Resouce Identifier > URL : location, URN : Name
- URN은 자주 사용되지 않아서 보통 URI와 URL을 같은 개념으로 이해함.

* URL 전체 문법
- scheme://[userinfo@]host[:port][/path][?query][#fragment]
- scheme : 프로토콜 정보(http, https)
- userinfo : 사용자 정보 포함 인증(거의 사용 안함)
- host : 호스트명, 도메인명/IP 주소.
- port : 접속포트, 일반적으로 생략(http:80, https:443)
- path : 리소스 경로 ex) ./home/file.jpg
- query(query string): key=value 형태, ?로 시작, &로 추가.
- fragment : html 내부 북마크

* 웹브라우저 요청 흐름
1. url 입력하고 확인, HTTP 요청 메시지 생성함
2. socket 라이브러리를 통해 전달(TCP/IP 연결, 데이터 전달)
3. TCP/IP 패킷 생성(HTTP 메시지 포함), 서버에 전달
4. 서버에서 받아서 응답 패킷 생성 및 전달
5. 웹브라우저에 html 렌더링.

* 2. HTTP(Hyper Text Transfer Protocol)
- 모든 것이 HTTP : 거의 모든 형태의 데이터를 전송할 수 있음.
- HTML, Image, API 등등.
- 서버간, 서버-클라이언트 단의 메시지로 이용됨.
- HTML/1.1을 표준으로 사용.(HTML2, HTML3는 내용이 조금만 다름.)

* HTTP 특징
- 클라이언트 서버 구조
- 무상태 프로토콜(Stateless), 비연결성
- HTTP 메시지
- 단순하고 확장성이 높음.

* 2-1. 클라이언트 서버 구조
- Request Response 구조
- 클라이언트 : 서버에 요청을 보내고 응답대기
- 서버 : 요청에 대한 결과를 만들어서 응답
- 클라이언트와 서버가 서로 독립적, 개발자는 서버만 잘 관리하면됨.

* 2-2. Stateless
- 서버가 클라이언트의 상태를 보존안함
- 이로인해 중간 장애나 제약이 적어서버의 확장성이 매우 높음
- 상태유지는 최소화 해야하나 필요한 경우 쿠키, 세션을 이용해 상태 유지.

* 2-3. 비연결성
- 요청시에만 서버와 클라이언트의 연결 유지.
- 계속 연결하지 않으므로 최소한의 자원만 사용.(효율적)
- 3way handshaking으로 인해 한번에 정보 받아오는게 어려움(비효율적)
- 이를 해결하기 위해 지속연결 이용(필요한 데이터 모두 받을때까지 연결 유지)

* 2-4. HTTP 메시지
- 시작 라인, 헤더, 공백라인, body의 구조를 가지고 있음.
- 요청 시작 라인 : http 메서드, 요청 대상, http vesion
- 응답 시작 라인 : http vesion, 상태 코드
- http header : http 전송에 필요한 모든 메타 정보.
- 메시지 body : 실제 전송할 데이터