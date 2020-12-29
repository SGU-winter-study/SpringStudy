## 0. 시작하기 전에

- JAVA 11 설치

- IDE 설치 (Intelij 사용)
- [https://start.spring.io/](https://start.spring.io/) (Spring boot를 기반으로 Spring 프로젝트를 만들어주는 사이트)

 * Project: Gradle Project
 * Language: Java
 * Project Metadata: Artifact = 프로젝트 이름
 * Dependencies: Spring Web, Thymeleaf


위의 설정 적용 후 generate하면 zip 파일이 다운로드된다. 해당 압축을 풀고 설치한 IDE에서 project로 "build-gradle" 파일을 열면 된다.

spring initializr 에 의해 기본적인 환경 세팅이 다 되어 있는 상태이기 때문에 별다른 추가 조치 없이 src/main/java/(프로젝트이름)Application 파일을 연 후 main 함수가 생성되어 있는지 확인하고, 해당 함수를 run 했을 때 콘솔에

`[     main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)`

이런 문구가 뜨면 성공이다. (localhost:8080 에 접속해보자! Whitelabel Error Page가 뜨면 정상적으로 작동하는 것.)

- - -

## 1. Welcom Page 생성
docs.spring.io 페이지에 들어가면 spring에 대한 기본적인 설명을 볼 수 있다. 먼저 Welcom page 에 대한 부분을 발췌하면 다음과 같다.

> Spring Boot supports both static and templated welcome pages. It first looks for an index.html file in the configured static content locations. If one is not found, it then looks for an index template. If either is found, it is automatically used as the welcome page of the application.

Spring boot는 시작과 동시에 index.html 파일을 탐색한다고 한다. 그렇기 때문에 우선 우리가 해야 할 일은 **src/main/resources/static** 폴더 내에 **index.html** 파일을 생성하는 것이다.

이 때, 경로에서 알 수 있듯이 static 폴더는 정적 파일들을 저장하기 위한 폴더이다. 정적 파일이라함은, 내가 작성한 파일을 웹 서버(tomcat)가 해당 텍스트를 그대로 가져다가 옮겨준다는 것이다. 반면, 템플릿 엔진(thymeleaf)을 사용하면 무언가 '프로그래밍'을 할 수가 있다.


#### 1.1 웹 서버에서 데이터를 보내는 방식

- **Static Contents**
	텍스트 그대로 raw html 파일을 전달하는 것이다. src/main/resources/static 폴더 내부에 *.html 파일로 저장되어 있는 html 파일을 별다른 처리 없이 그대로 전달한다. 즉 프로그래밍 되지 않은 상태의 데이터이다.
    
- **MVC (Template Engine)**
	렌더링된 html을 전달한다. Model, View, Controller를 이용하여 변수를 사용하거나 loop을 이용하여 코드를 작성하는 등 프로그래밍이 가능하다. 이렇게 작성한 프로그램에 대한 연산과 처리가 모두 적용된 View의 결과를 사용자에게 보여준다.
    
- **API**
	JSON 형식으로 객체를 반환한다. return 으로 지정된 값을 텍스트 형태 그대로 화면에 보여준다.
    
    
#### 1.2 MVC
- **Model**
	필요한 데이터를 View로 넘겨준다. 일종의 DB와 같은 역할을 수행한다.
    
- **View**
	사용자에게 보일 화면을 구성한다.
    
- **Controller**
	프로그래밍된 연산을 수행한다.

- - -

## 2. View 생성

View는 사용자가 url을 통해 웹 페이지에 접속했을 때 화면에 보여줄 데이터(텍스트, 이미지 등)를 구성하는 파일이다.
**src/resources/templates** 내부에 *.html 의 형태로 생성한다. 

``` HTML 
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >Welcome</p>
</body>
</html>
```
이 때, p tag 내부의 ${data} 는 Controller에 의해 Model에서 data라는 속성을 가진 value가 대신 채워지게 된다. 만약 매칭되는 데이터가 없거나 Controller가 없다면 에러가 발생하게 된다. 따라서 Controller를 추가해 주어야 한다.

※ **th:text** 는 thymeleaf 라이브러리 문법으로, 해당 태그 내의 값("Welcome")을 다른 텍스트로 대치하는 역할을 수행한다. 따라서 실제로 화면에는 "Welcome" 대신 "안녕하세요. (data에 해당하는 value)" 가 보여지게 된다.

- - -

## 3. Controller 생성

**src/main/java** 하위의 프로젝트 폴더에 새로운 Package를 생성하여 controller를 생성해준다. 그리고 해당 Package 아래에 controller라는 이름의 java class를 생성한다. 

Controller는 사용자가 입력하는 url을 바탕으로 적절한 View에 필요한 데이터를 넘겨주고 연결하는 역할을 수행한다.


``` java
@Controller
public class MyController {
    @GetMapping("home")
    public String home(Model model) {
        model.addAttribute("data", "hello!");
        return "hello";
    }
}
```


* **@Controller**
	해당 클래스가 Controller임을 명시하는 annotation이다. Controller를 정의하기 위해서는 반드시 쓰여야 한다.
    
* **@GetMapping("*myUrl*")**
	어떤 url과 매핑되는지에 대한 정보를 나타낸다. 기본 url(로컬 서버의 경우 http://localhost:8080) 뒤에 붙게 될 url 주소를 넘겨준다. (이 경우 localhost:8080/myUrl 에 매핑되는 Controller이다.)
    
* **public String home(Model model)**
	String 을 return하는 home이라는 method를 정의한다. MVC 방식을 사용하기 때문에 parameter로 모델(Model)을 넘겨준다.
    
* **model.addAttribute("*data*", "*hello!*");**
	parameter로 받은 모델 model에 attribute를 추가한다. 이때 첫 번째 parameter는 attribute의 이름(attributeName)을 나타내고 두 번째 parameter는 attribute의 값(attributeValue)을 나타낸다.
    
* **return "hello";**
	"hello"라는 String을 반환하도록 한다. 이 String은 View Resolver에 의해 자동으로 동일한 이름을 가진 html 파일로 연결된다. 즉, resources/template/hello.html 이라는 파일로 해당 Controller는 데이터를 보내주게 되는 것이다.
    
즉 위의 코드는 "localhost:8080/myUrl"을 통해 접속하게 되었을 때 "resources/template/hello.html" 이라는 파일을 화면에 보여주도록 하는 역할을 수행하는 Controller라고 할 수 있다. 
