<h1> 프로젝트 환경설정</h1>
스프링 부트 스타터를 이용해서 설정을 한 뒤에 스프링 프로젝트를 생성한다.

marven,gradle 프로젝트가 있는데, 현재 거의 모든 프로젝트에서 gradle을 사용한다.

필요한 라이브러리를 가져오고 프로젝트의 라이프사이클을 관리해주는 역할을 하고, 간단하게 프로젝트에서 필요한 것들의 버전을 설정하고 필요한 라이브러리를 땡겨오는 것이라고 생각하면 된다.

dependecies에 Thymeleaf 와 같은 템플릿 엔진을 추가해준다.
  이는 안드로이드 프로그래밍에서 파이어베이스나 인터넷을 이용할 때, build.gradle에다가 dependencies를 입력에서 sync 맞춘 것이랑 비슷하게 생각.
  django에서 MVT 패턴 이용해서 개발할 때, view 보여주는 template를 자동으로 생성해주는 것이라고 생각하면 편함.
  
spring initailzr을 이용해서 만든 프로젝트를 intelliJ에서 열어줄 때, Open하면서 다운로드한 폴더 안의 .gradle 파일 오픈하면서 as project로 열어주면 된다.

필요한 레퍼런스는 docs.spring.io를 이용하면 되고, 지금 진행하는 프로젝트는 2.4.1 버전이니까 https://docs.spring.io/spring-boot/docs/2.4.1/ 를 참고하면 된다.

먼저, 스프링 부트는 웹 브라우저와 스프링 컨테이너 사이에 tomcat 서버를 두고있다.

tomcat 서버는 요청이 들어오면 들어온 요청을 컨테이너에 던져준다. 

웹 브라우저에서 요청이 들어오면 3가지 중 한가지를 이용해서 처리를 한다.
1. 정적 컨텐츠(static contents) : 있는 그대로를 보여줘야하는 경우
2. MVC : 처리된 결과를 랜더링해서 html 형태로 클라이언트에게 전달해야하는 경우
3. API : 객체를 전달해야하는 경우

<h2>static contents</h2>
독립형 웹 어플리케이션에서 컨테이너의 default servlet이 enable하고 fallback으로서의 역할도 할 수 있는 경우에 ServletContext의 root로 부터 컨텐츠를 가져온다. 
이때 이 컨텐츠는 스프링에서 handle하지않는 것이여야함. 그 이유는 스프링 컨테이너에서는 컨트롤러가 우선 순위를 가지기 때문이다. 그래서 일단 http 요청이 들어오면 해당 컨트롤러가 있는지 확인한 후에 다음으로 진행된다.
 
스프링 부트에서는 static content를 불러올 때, /static에서 불러온다. 이는 프로젝트 안에 생성이 되어져있다.
 
<h2>MVC</h2>
mvc 패턴은 model, view, controller의 앞글자만 가져온 것

관심사의 분리를 해준 것이다.
model : 프레젠테이션 계층의 구성요소 정보
view : 화면 출력 로직
controller : 제어 로직(비즈니스 로직이나 내부적인 처리 로직), 서버 뒷단쪽 처리
http 요청이 들어오면 프론트컨트롤러에서 컨트롤러에 요청

mvc를 이용해서 http 요청을 처리하는 일련의 과정은 다음과 같다.
1. 컨트롤러는 해당 요청에따라서 모델을 생성, 컨트롤러는 모델과 뷰를 프론트 컨트롤러에 돌려준다.
2. 그러면 프론트 컨트롤러는 모델을 참조해서 뷰를 생성한다.
3. 이렇게 만들어진 뷰로 HTTP 요청에대한 응답을 한다.

템플릿 엔진을 이용하는 경우에는 컨트롤러에서 리턴값으로 문자를 반환하면 viewResolver가 찾아서 처리를 해준다.
스프링 부트 템플릿엔진 기본 viewName 매핑을 이용한다.
`resource:template/`+{viewName}+'.html'로 자동으로 연결해줘서 처리를 한다.


@어노테이션
  
  @RequestParam : 외부에서 파라미터를 받아오겠다는 의미로 http 요청에서 get을 할 때, ?,= 을 통해서 확인할 파라미터를 받을 때 사용하는 것 같다. 존재하지않는 경우에 BadRequest로 http 4**에러 발생
  

<h2>API</h2>
@어노테이션
  
  @ResponseBody - http의 body 부분에 직접 이 어노테이션이 붙은 메소드의 반환값을 넣어주겠다는 의미. - 소스보기로 들어가면 입력한 것만 그대로 넘어가서 그 데이터만 보여준다.
웹 브라우저에서 요청한 데이터가 객체인 경우에는 json 형태로 넘겨준다.

   JSON - JavaScript Object Notation
    
    자바스크립트에서 오브젝트를 사용하는 방법을 여러 프로그래밍 언어와 환경에서 표준으로 사용할 수 있게 만들어놓은 것
    
API는 http 요청에대한 응답으로 데이터를 그대로 넘겨줘야하는 경우에 사용한다.

mvc랑 다른 점은 mvc에서는 요청 들어오면 그에 맞는 view를 viewResolver에서 처리하는데,
api는 HttpMessageConverter가 동작해서 처리함

단순 문자면 StringConverter
객체면 JsonConverter가 작동해서 - 기본 객체 처리는 스프링에서 MappingJackson2HttpMessageConverter를 이용해서 처리해줌.
여러가지 처리가 기본으로 등록되어있음. 실무에서 거의 그대로 사용함.

HTTP Accept 헤더에서 요철할 때 데이터 타입을 설정해서 받을 수 있다.
이렇게 데이터 타입을 설정해서 요청하면 그에 대응되는 컨버터가 작동해서 반환해준다.
  node.js express를 사용하는 경우에는 다 install하고 require 해줬어야하는데 그런 불편함이 사라지는 것 같다.

<h2>단축키 및 기타 개발 tip</h2>
  command N - getter and setter 등을 생성할 수 있는 카테고리를 보여준다.
 
    getter, setter 는 private 데이터에 접근할 때 사용한다.
    
  command B - 에러 처리 방법을 보여줌 (ex, import class~~)
  
  command P - default 값을 보여주는 단축키
  
  System.out.print()를 이용하는 것보다 log()를 찍어줘서 에러를 관리해줘야한다. 왜냐하면 log에는 로그가 찍힌 시간 같은 유용한 정보가 들어있다.

