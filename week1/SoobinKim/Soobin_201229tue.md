##Review
* 컨트롤러: 웹 MVC의 컨트롤러 역할
* 서비스: 핵심 비즈니스 로직 구현
* 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
* 도메인: 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

- - -
##테스트케이스 작성
* JUnit이라는 framework를 이용하여 테스트케이스 작성
* src/test/java 폴더에 테스트 코드 파일 생성
* 테스트는 각각 독립적으로 실행되어야 함 -> 테스트 실행 후 DB 초기화 작업 필요
* 테스트 함수명은 가독성을 위해 한글로 작성해도 무방하다.
* given when then
* DI(Dependency Injection)
	- 개발자에 의해 직접 주입하는 방식(parameter로 넘겨줌)이 있고, @Autowired를 이용하여 스프링이 스프링 컨테이너에서 찾아 주입하도록 하는 방식이 있다.
	- 생성자에서 주입하는 방식과 필드 주입 방식, setter 주입 방식의 세 가지가 있다. (생성자 주입 권장)
* @Test
* @AfterEach (import org.junit.jupiter.api.AfterEach;)
* @BeforeEach (import org.junit.jupiter.api.BeforeEach;)


값이 있는지 없는지 모르는 결과에 대해서는 Optional로 감싸서 반환해주면 된다.
Optional<Member> result = MemberRepository.findByName(member.getName());
result.ifPresent(--)
이런 식으로 result가 값이 있는 경우(null이 아닌 경우)에 대해서만 동작을 수행할 수 있다.
(=> if != null 과 같은 식으로 처리하지 않아도 됨)

코드를 정리하면,
MemberRepository.findByName(member.getName()).ifPresent(--);
이런식으로 한 번에 처리하는 것이 더 깔끔하다.

_ _ _

##스프링 빈
이제 메모리 DB를 바탕으로 데이터를 화면에 표현하기 위해서는 Controller와 View가 필요하다.
Controller 파일에서 @Controller 라는 annotation을 보고 Spring이 이를 객체로 생성하여 가지고 있게 됨. = 이를 "Spring Controller에서 스프링 빈이 관리된다"라고 한다.
스프링 빈을 등록하는 방법은 크게 두 가지: 1) 컴포넌트 스캔과 자동 의존관계 설정 2) 자바 코드로 직접 스프링 빈 등록
1) annotaion을 사용하여 컴포넌트 스캔을 하는 방법:
@Autowired 라는 annotation을 생성자에 붙임으로써 스프링 컨테이너에서 해당 객체를 가져와서 사용하게 된다(Dependency Injection; 의존관계를 주입함). 이 때 서비스의 객체를 사용하기 위해서는 스프링이 이를 인지하고 스프링 컨테이너에 가지고 있어야 하므로, 이를 스프링에게 알려주기 위해 서비스 객체 위해 @Service 라는 annotation을 함께 붙여주어야 스프링이 이를 인식할 수 있다. 마찬가지로 리포지토리 객체에도 @Repository 라는 annotation을 붙여주어야 한다.
=> 정형화된 controller, service, repository 코드에 사용이 편리하다

2) 자바 코드로 직접 스프링 빈 등록하기
SpringConfig 라는 파일을 생성한 후 다음과 같이 @Bean annotation을 이용하여 내용을 채운다.
``` -Java
@Configuration
public class SpringConfig {
	@Bean
	public MemberService memberService() {
		return new MemberService(memberRepository());
	}
	@Bean
	public MemberRepository memberRepository() {
		return new MemoryMemberRepository();
	}
}
```
=> 상황에 따라 구현 클래스를 변경해야 하는 경우에 유용하다. (SpringConfig 파일만 수정해주면 됨)

- - -

##웹 MVC 개발

Controller의 annotation 우선순위
1. 스프링 컨테이너 내부에 Controller 존재하는지 여부 확인
2. 스프링 컨테이너 내부에 Controller가 없다면 정적 파일에서 탐색

