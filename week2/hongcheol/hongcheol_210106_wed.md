# 컴포넌트 스캔과 의존관계 자동 주입

<br>@componentScan 을 이용해서 사용할 거 다 끌어온다.
<br>-@component 가 붙은 애들을 다 찾아서 스프링에 등록을 해준다.
<br>@Component(“이름”)과 같은 방식으로 이름을 부여할 수 있다.
<br>@componentScan을 사용하면
<br>@Bean으로 등록한 클래스가 하나도 없다.

의존관계 주입은 Autowired를 이용해서 자동으로 해준다. 위치는 생성자 위!
<br>@Autowired는 다양한 기능이 있다.
<br>그 중 하나가 자동으로 getBean( )메소드를 사용해주는 것이다.

Autowired는 기본 조회 전략은 타입이 같은 빈을 찾는 것이다. 아무리 파라미터가 많아도 알아서 잘 찾아서 자동으로 주입해준다.

## 탐색 위치와 기본 스캔 대상

basePackages = “시작위치“으로하면 시작위치와 그 하위항목들을 스캔한다.
<br>위치를 지정하는 이유 - 라이브러리까지 다 뒤지니까 필요한 것만 탐색하기 위해서

권장하는 방법
<br>패키지 위치를 지정하지않고, 현재 프로젝트의 최상단을 설정 정보 클래스의 위치로 두는 것이다.

그러면 내 프로젝트 관련된거는 자동으로 다 뒤지게되는 것이다. 이는 프로젝트의 메인 설정 정보는 프로젝트를 대표하는 정보이기 때문에, 프로젝트 시작 루트 위치에 두는 것이 좋다.

스캔 기본 대상
<br>@Component : 컴포넌트 스캔
<br>@Controller : MVC 컨트롤러
<br>@Service : 비즈니스 로직에서 사용
<br>@Reposiroty : 데이터 접근 계층(JPA나 JDBC 등등)
<br>@Configuration : 스프링 설정 정보에서 사용

애노테이션끼리는 상속관계가 없다. 

각 애노테이션이 붙으면 스프링은 다음과 같은 부가 기능을 수행해준다.
<br>컴포넌트 스캔의 용도 뿐만 아니라 다음 애노테이션이 있으면 스프링은 부가 기능을 수행한다. 
<br>@Controller : 스프링 MVC 컨트롤러로 인식
<br>@Repository : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다. 
<br>@Configuration : 앞서 보았듯이 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다.
<br>@Service : 사실 @Service 는 특별한 처리를 하지 않는다. 대신 개발자들이 핵심 비즈니스 로직이 여기에 있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다. 


필터
<br>컴포넌트 스캔 대상에 추가하거나 제외할 대상을 지정할 때 사용
<br>FilterType은 5가지 옵션이 있다. 
<br>ANNOTATION: 기본값, 애노테이션을 인식해서 동작한다. 
<br>ex) org.example.SomeAnnotation 
<br>ASSIGNABLE_TYPE :지정한 타입과 자식 타입을 인식해서 동작한다.  
<br>ex) org.example.SomeClass 
<br>ASPECTJ : AspectJ 패턴 사용
<br>ex) org.example..*Service+ 
<br>REGEX: 정규 표현식
<br>ex) org\.example\.Default.* 
<br>CUSTOM: TypeFilter 이라는 인터페이스를 구현해서 처리 
<br>ex) org.example.MyTypeFilter 

중복 등록과 충돌
<br>컴포넌트 스캔에서 중복된 이름의 빈이 등록되면 충돌이 일어난다.
<br>자동 vs 자동
<br>절대 안된다.
<br>ConflictingBeanDefinitionException 이 발생한다.  자동으로 하는 경우에는 충돌이 일어나는 경우가 없다.

<br>수동 vs 자동
<br>수동빈이 우선권을 가진다.
