# 컴포넌트 스캔
> 컴포넌트 스캔: 설정 정보 없이도 자동으로 스프링 빈을 등록하는 기능

## 의존관계 자동 주입
@ComponentScan
: @ComponentAnnotaion이 붙은 클래스를 찾아 전부 자동으로 스프링 빈에 등록해준다. 이 때, 스프링 빈의 기본 이름은 클래스 이름과 동일하게, 맨 앞 글자만 소문자로 바꾸어 생성된다. (@Component에 인자를 줌으로써 직접 이름을 지정할 수도 있다.)

@Autowired
: 자동 의존관계 주입 기능. 스프링이 알아서 맞는 타입을 스프링 컨테이너에서 찾아 와서(타입으로 찾음) 자동으로 주입해준다.
= getBean(*.class) 와 작동 방법이 유사하다.

## 탐색 위치와 기본 스캔 대상
- basePackage: 탐색할 패키지의 시작 위치를 지정할 수 있다. 지정한 패키지를 포함하여 모든 하위 패키지를 탐색한다. 여러 개의 시작 위치를 지정하는 것도 가능하다. 필요한 클래스만 탐색하게 함으로써 속도를 빠르게 할 수 있다.
- basePackageClasses: 클래스를 통해 탐색할 패키지를 지정한다.

만약 탐색할 패키지 정보를 지정하지 않은 경우 해당 설정 정보 클래스가 위치한 패키지가 시작 위치로 기본 설정 된다.
=> 그렇기 때문에 설정 정보 클래스를 프로젝트 최상단에 두는 것이 관례상 권장된다.

ComponentScan은 다음 내용들을 대상에 포함한다.
- @Component
- @Controller
- @Service
- @Repository
- @Configuration

실제로 해당 annotation들을 열어보면 상위에 @Component annotation이 붙어 있음을 확인할 수 있다.
※ annotation(일종의 메타 정보)은 상속 관계를 갖지 않기 때문에 이렇게 annotation이 annotation을 갖는 것은 Java 언어 그 자체로는 인식이 불가능하고, 스프링에서 해당 기능을 지원하기 때문에 가능하다.

## 필터
ComponentScan의 인자로 includeFilters, excludeFilters 필드를 통하여 스캔에 포함할 필터, 스캔에서 제외할 필터를 걸 수 있다. (excludeFilters 에 포함된 annotation이 붙은 클래스는 컴포넌트 스캔 대상에서 제외되어, 스프링 빈에 등록되지 않는다.)
```Java
@ComponentScan(
	includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
	excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
)
```
FilterType에 존재하는 옵션은 다음과 같다.
* ANNOTATION
: 기본값. Annotation 인식을 통해 동작한다.
* ASSIGNABLE_TYPE
: 지정한 클래스 타입과 자식 타입 인식을 통해 동작한다.
* ASPECTJ
: AspectJ 패턴을 사용한다.
* REGEX
: 정규 표현식을 사용한다.
* CUSTOM
: 직접 TypeFilter라는 인터페이스를 구현하여 사용한다.

## 중복 등록과 충돌
스프링 빈의 이름이 중복되는 경우 충돌하여 에러가 발생한다.
자동 빈 등록의 경우 클래스 명과 동일하게 스프링 빈 이름이 생성되기 때문에 충돌할 일이 없으나, 수동 빈 등록의 경우 중복 충돌이 발생하지 않도록 주의해야 한다.
만약 자동으로 등록된 빈과 수동으로 등록한 빈의 이름이 충돌하는 경우 수동으로 등록된 빈이 우선권을 가져, 수동 빈이 자동 빈으로 overriding한다.
```text
Overriding bean definition for bean '*' with a different definition: replacing
```
