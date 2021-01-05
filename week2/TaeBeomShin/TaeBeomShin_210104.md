<h1>스프링 핵심원리 - 3일차(스프링 컨테이너와 스프링 빈) </h1>

## Section 4 스프링 컨테이너와 스프링 빈

### 스프링 컨테이너 생성

-  ApplicationContext : 스프링 인터페이스, AnnotationConfigApplication(AppConfig.class) : 구현체

- 스프링 컨테이너는 XML을 통하거나 어노테이션을 통해서 만들 수 있다.(형식만 다를뿐 내용은 동일함)

* 스프링 컨테이너 생성 과정

    1. 스프링 컨테이너 생성 : 말그대로 컨테이너를 생성하고, 정보목록을 지정한다.
    2. 스프링 빈 등록 : 정보를 받아서 생성한 컨테이너에 등록 시킨다.(빈의 이름은 unique 해야한다.)
    3. 스프링 빈 의존관계 설정 - 준비
    4. 스프링 빈 의존관계 설정 - 완료 : 설정 정보를 통해 의존관계를 주입한다.

### 스프링 빈 조회

- 모든 빈 출력하기 : getBeanDefinitionNames(), getBean()

- 어플리케이션 빈 출력하기(내가 등록한 것만): ROLE_APPLICATION, ROLE_INFRASTRUCTURE

- 빈 이름으로 조회 : getBean()

- 동일한 타입의 모든 빈 조회 : getBeansOfType()

- 동일한 타입 중 하나만 : getBean()에 빈 이름을 넣어서 호출하면 됨.

- 상속관계 부모타입 중 하나 : getBean()에 호출하려는 자식이름 넣어서 호출.

- 상속관계 모두 호출 : getBeansOfType() 이용.

### BeanFactory와 ApplicationContext

: 둘 다 스프링 컨테이너 이며, BeanFactory가 ApplicationContext 더 상위에 있는 
클래스이고, BeanFactory가 기본적인 기능 (관리, 조회<- 위의 예시들)을 담당하고, 
ApplicationContext가 부가기능(환경변수, 리소스 조회 등)을 추가한 컨테이너다

### 스프링 빈 메타 정보(BeanDefinition) 

:BeanDefinition 이라는 메타 정보 안에 스프링 빈 설정(XML, Config)정보와 다양한 메타정보가
(빈 클래스명, Scope, factoryMethodName) 등이 들어 있어서 스프링 빈은 다양한 설정 형식을
사용자에게 지원할 수 있다.

- 추가적으로 BeanDefinition을 직접 정의 하는 것도 가능하다.