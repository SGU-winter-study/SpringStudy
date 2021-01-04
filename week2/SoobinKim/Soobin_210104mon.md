## Intellij 단축키 (Windows)

* **ctrl + alt + Enter** = 명령어 완성
(세미 콜론 붙여주거나 이미 완성된 명령어인 경우 커서 문장 끝으로 이동시키지 않아도 다음 줄로 줄바꿈)
* **ctrl + alt + v** = 변수 생성
new 를 이용하여 새로운 객체를 생성하였을 때 앞에 자동으로 변수를 생성해준다.
* **alt + Enter** = 자동 import
* **alt + insert** = 메소드 자동 생성
	* Constructor
	* Setter
	* Getter
	* toString()
	* ...
* **F2** = 에러 발생 구문으로 자동 이동
* **ctrl + d** = 구문 복사 + 붙여넣기
* **ctrl + shift + F10** = 실행
* **ctrl + shift + T** =  Test 생성
* **ctrl + E** = 히스토리
* **ctrl + alt + M** = 메소드 추출
* **ctrl + r** = 직전에 실행했던 작업 재실행
* **soutv** = 변수 출력
* **iter** = 자동으로 for loop 생성

_ _ _

## Spring Container
@Configuration, @Bean 이라는 annotatino을 통해 설정 정보 저장. (해당  annotation으로 인해 @Bean이 붙은 객체들을 spring 컨테이너 내부에서 관리)

이제, AppConfig에 직접 접근하여 내부 객체들을 불러오는 것이 아니라 **ApplicationContext**를 이용하여 스프링 컨테이너에 접근한 후 내부에 저장되어 있는 객체들을 가져오게 됨.

```Java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
```
여기서 **ApplicationContext**를 스프링 컨테이너라고 한다.
ApplicationContext는 인터페이스이고, 이를 구체화하여 구현한 것이 **AnnotationConfigApplicationContext**이다. (인자로 1. 메소드(Bean)이름, 2. 반환값 을 보낸다.)


#### 스프링 빈 조회
Bean은 타입으로 조회할 수도 있고, 특정 타입을 가진 Bean이 여러 개 존재하는 경우 이름으로 찾을 수도 있다.
또한 Bean에는 상속 관계가 있어, 부모 타입으로 조회하면 그 자식 타입들이 전부 함께 조회되게 된다. (Object 타입으로 조회하면 모든 스프링 빈을 조회하게 된다.)


#### BeanFactory와 ApplicationContext
스프링 컨테이너는 BeanFactory와 ApplicationContext 모두를 의미한다.
- BeanFactory
스프링 컨테이너의 최상위 인터페이스로, 스프링 빈을 관리하고 조회하는 역할 담당
- ApplicationContext
BeanFactory의 기능을 모두 상속받아서 제공한다.
이에 추가로 빈 관리기능과 기타 편리한 부가 기능을 함께 제공한다.
ex. 메시지소스를 활용한 국제화 기능, 환경변수, 애플리케이션 이벤트, 편리한 리소스 조회
따라서 대부분의 경우 우리는 ApplicationContext를 이용하게 된다.


#### 스프링 컨테이너를 사용하면:
* 기존에 Java 코드를 이용하여 직접 하나하나 수행하던 과정을, 스프링 컨테이너에 스프링 빈으로 객체를 등록함으로써(ex. MemberService, OrderService) 필요할 때마다 스프링 컨테이너에서 스프링 빈을 찾아 사용하도록 할 수 있다.


## XML 설정 사용
기존의 annotation을 기반으로 한 자바 코드는:
* new AnnotationConfigApplicationContext(AppConfig.class)
* AnnotationConfigApplicationContext
이 두 가지의 클래스를 활용하여 설정 정보를 넘겨주는 작업을 수행하였다.

반면 XML은, 자바 코드가 아니라 XML이라는 하나의 문서를 통해 설정 정보를 사용하는 것이다.
```XML
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
    </bean>
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository" />
```
이렇게 자바 코드에서와 동일하게 문서 형태로 존재한다.

## BeanDefinition
위에서 자바 코드 또는 XML로 설정 정보를 저장하는 방법에 대하여 알아보았다.
스프링에서 자바 코드로 작성된 정보든, XML로 작성된 문서 형태의 정보든 상관 없이 객체들을 스프링 컨테이너에 저장할 수 있는 이유는 **BeanDefinition**이라는 것이 존재하기 때문이다.
**BeanDefinition**이란 **빈 설정 메타정보**로, 자바 코드의 경우 **@bean**, XML의 경우 **\<bean>** 을 통해 인식하여 각각 하나씩 메타 정보가 생성되게 된다. 스프링 컨테이너는 이러한 메타 정보를 기반으로 스프링 빈을 생성한다.
