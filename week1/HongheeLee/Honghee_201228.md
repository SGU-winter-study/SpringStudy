## 섹션 1. 프로젝트 환경설정

​	IDE는 Eclipse나 IntelliJ를 사용하면 되지만 최근엔 IntelliJ를 많이 사용한다. 또한 예전에는 스프링 프로젝트를 하나하나 타이핑해서 설정한 이후에 생성할 수 있었지만 최근에는 스프링부트를 통해 스프링 프로젝트 간단히 생성할 수 있다.

​	최근에는 Gradle Project로 대부분 이루어진다. Gradle은 프로젝트 빌드관리도구로 다양한 라이브러리를 모두 다운받을 필요 없이 빌드도구 설정 파일에 필요한 라이브러리 종류와 버전들 종속성 정보를 명시하여 필요한 라이브러리들을 설정파일을 통해 자동으로 다운로드해주고 이를 간편하게 관리하게 도와준다.

​	여기서는 Dependencies로 Spring Web과 Thymeleaf를 설정했다. Gradle은 의존관계가 있는 라이브러리를 함께 다운로드하기 때문에 tomcat이나 web-mvc 등의 라이브러리도 같이 설치되고 공통적인 요소인 starter와 test 라이브러리도 설치된다. 라이브러리의 의존성관계는 IntelliJ의 Dependencies 탭에서 확인할 수 있다.

​	스프링부트에서는  src/main/resources/static/index.html을 올려두면 Welcome Page 기능을 제공한다. 또한 src/main/java는 자바코드(컨트롤러, 모델), src/main/resources는 자바 코드에서 사용할 리소스들, src/test/java는 테스트 코드, src/test/resources는 테스트 코드에서 사용할 리소스를 저장한다.

---

## 섹션 2. 스프링 웹 개발 기초

- 정적 컨텐츠

  ​	써져있는 그대로 넘겨줘서 보여준다. 브라우저에서 내장 톰캣 서버로 넘어온 값에 관련된 컨트롤러가 스프링 컨테이너에 없을 때 그 다음엔 resources/static에서 관련된 html 파일을 찾는다. 있으면 그걸 웹브라우저에 보내서 그대로 띄우게 한다. 

- MVC와 템플릿 엔진

  ​	HTML을 내리는 방식. MVC는 Model, View, Controller의 약자이다.  과거에는 Controller와 View가 구분하지 않고 View에 모든 기능을 다 넣어 프로젝트를 구현하기도 했지만 지금은 관심사를 분리하기 위해 View는 화면을 보여주는 역할, Controller는 비지니스 로직을 담당하는 역할로 구분해서 구현한다.  또한 Controller는 Model에 View에서 필요한 정보를 넣어서 넘겨준다. 

  ​	Controller에서 리턴 값으로 문자를 반환하면 viewResolver가 resource/templates/+{ViewName}+.html를 찾고 model에 담겨 넘어온 데이터로 Thymeleaf 템플릿 엔진 처리를 한다. 보통 ${key}자리에 value를 넣어 업데이트 하는 방식이다. 이 HTML을 웹 브라우저에 넘겨 볼 수 있게 된다. 

- API

  ​	데이터를 그대로 내리는 방식. 문자을 그대로 반환할 수도 있고, 객체를 반환할 수도 있는데 객체를 반환하면 객체가  {key:value} 형태로 이루어진 JSON으로 변환되어 반환된다.  @ResponseBody 어노테이션을 사용해서 viewResolver를 사용하지 않고 HTTPS의 BODY 위치에 이 내용을 직접 넣어주겠다는 것을 표시해준다. 주로 아래와 같이 객체를 반환하는 형태로 사용된다. 또한 필드 값에 접근할 때는 필드를 private 접근제한자로 설정하고 get~(), set~() 메소드로 접근하도록 한다. 

  ```java
  @Controller
  public class HelloController {
  	
      @GetMapping("hello-api")
  	@ResponseBody
  	public Hello helloApi(@RequestParam("name") String name) {
  		Hello hello = new Hello();
  		hello.setName(name);
  		return hello;
  	}
  	static class Hello {
  		private String name;
  		public String getName() {
  			return name;
  		}
  		public void setName(String name) {
  			this.name = name;
  		}
  	}
  }
  ```

  ---