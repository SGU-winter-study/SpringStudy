<h1>스프링 핵심원리 - 1일차(객체 지향 설계와 스프링) </h1>
본 강의에 들어가기전 간략히 스프링이 핵심 원리와 객체지향 이론에 대해 살펴본다.
스프링을 프레임워크로서 기능만 활용할 줄 아는 게 아니라, 제대로 이해하고 사용하기 위해 필요.

## Section1 객체 지향 설계와 스프링

### 스프링 역사

- EJB를 이용한 개발 -> Spring과 Hibernate -> JPA(자바표준):Hibernate + Eclipse Link + ...
- 스프링 프레임워크와 더불어 스프링 부트의 개발.

### What is Spring?

1. About Spring System.
    - Core : 스프링 프레임워크, 스프링 부트
    - 선택 : 스프링 데이터, 스프링 세션, 스프링 세큐리티, 스프링 배치 ...

2. Core System.
    - Spring FrameWork : 스프링 DI 컨테이너, AOP, 등등.
    - Spring Boot : 스프링을 편리하게 사용할 수 있도록 지원(Starter을 통한 종속성 설정, 내장 Tomcat 서버)

3. Meanings of Spring
    Spring이라는 단어가 너무 범용성 있게 사용되어 헷갈렸는데 이 부분의 정리로 어느정도 이해가 된 듯.
    보통 아래의 3가지 중 하나를 의미한다.

    - 스프링 DI 컨테이너 기술.
    - 스프링 프레임워크
    - 스프링 생태케

### Why we need Spring?

스프링이 왜 필요한지 이해하기 위해서는 스프링의 핵심 concept이 무엇인지 파악해야한다. 

스프링은 객체지향형 언어인 자바가 "좋은 객체지향 어플리케이션" 을 개발할 수 있또록 하는 것이 목적이다.

즉, 스프링이 왜 필요하고 기능들이 어떤 의미인지 이해하기 위해서는 먼저 객체지향에 대해 이해하고, 좋은 

객체지향이 무엇인지 파악하는 것이 중요하다.

1. Objected Oriented 中 다형성
    - 다형성을 한마디로 정의하자면 역할과 구현으로 구분지어 개발하는 것이다. 이를 통해 클라이언트 사이드 에서의
    편의성을 높이고 서버의 단순성, 유연성을 높이는 것이 최종적인 목적이다.

    - 역할에 해당하는 부분은 인터페이스, 구현에 해당하는 부분은 클래스, 구현 객체이다.

2. 좋은 객체지향 5원칙 SOLID
    
    - SRP : 단일 책임 원칙, 하나의 클래스는 하나의 책임(변경시 파급효과가 적은 단위만큼만.)

    - OCP : 개방-폐쇄 원칙, 인터페이스의 변경은 불가능 하나, 클래스의 확장/생성은 가능하다.
    
    - LSP : 라스코프 치환의 원칙, 하위 클래스는 인터페이스와 동일한 목적성을 가진다.(의도에 맞게 오버라이딩 되어야한다.)

    - ISP : 인터페이스 분리 원칙, 인터페이스는 클라이언트별로 구체적으로 세분화 시켜야 한다.

    - DIP : 의존관계 역전의 원칙, 인터페이스(추상화)에 의존하되, 구현(구체화)에 의존해서는 안된다.

    - 이러한 SOLID를 기존 자바로는 준수할수 없기에 Spring이 필요하다.

### What do spring do?

-> 스프링은 DI컨테이너를 통해 OCP, DIP가 가능하게 지원해서, 클라이언트의 코드 변경없이 기능을 확장할 수 있도록한다.

즉, 객체지향적으로 프로그래밍 할 수 있도록 다양한 기능을 제공한다.