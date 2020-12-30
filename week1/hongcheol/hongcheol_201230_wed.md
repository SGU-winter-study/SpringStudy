# 웹 MVC
@GetMapping(“ “)
괄호 안에는 어디로 매핑할건지를 적어준다.
ex) /data면 localhost:8080/data 하면 그 어노테이션 붙은 것을 가져와서 보여준다.
template 만들 때 href 이용해서 원하는 기능하게 만들 수 있다.

우선 순위
요청이 오면 컨테이너에 매핑된게 있는지 찾아보고 없으면 정적 컨텐츠를 찾음.

GET 방식
그냥 적힌 곳으로 이동
url창에 치고 이동하는 것, 조회할 때 주로 사용

POST 방식
데이터가 이동해야할 때 주로 사용

${ } 안에 있는거는 model 안에 있는거 꺼낸다.

th:each <- thymeLeaf 문법 . 반복문 java의 forEach랑 비슷

메모리 안에 있기 때문에, 자바를 내리면 기존에 등록해놓은 것이 사라짐
-> 파일로 저장하거나 데이터베이스에 저장

# JPA
객체 + ORM (Object Relational database Mapping)
기존의 반복 코드 뿐만 아니라 기본적인 SQL도 직접 해줌.
객체 중심의 설계로 패러다임을 전환할 수 있다.

build.gradle 수정하고
application.properties에도 추가해야함.
sync 끝나면 jpa랑 hibernate 라이브러리가 들어와야함.


jpa 쓰려면 entity를 매핑해야함.

@Entity - JPA 가 관리하는 엔티티라는 것을 표시하는 것
@GeneratedValue(strategy = GenerationType./IDENTITY/)
IDENTITY는 DB가 알아서 생성해주는 id
django 에서 admin 페이지에서 데이터 넣어주고서 그거 삭제해도 id는 감소하지않았던 것을 생각하면 됨

JPA 를 쓰려면 EntityManager을 주입받아야한다.
persist() - 저장
find( class, column)
pk가 아닌 것들에대한 쿼리는 jpql이라는 것을 작성해야함.
-> createQuery(query, class) 객체를 대상으로 쿼리를 날리는 것
엔티티를 조회해서 객체 자체를 select하는 것 - mapping은 다 된 상태로 가져온다.
JPA는 모든 변경이 트랜잭션 안에서 수행되어야한다.

# DB 접근 기술
스프링 부트 데이터베이스 연결을 해줄 때,
spring.datasource.username=
을 안붙이면 테스트에서 통과를 못한다. 특히 = 뒤에 아이디에 공백이 들어가서 원래거랑 다르게되면 테스트 통과 못하니까 꼭 공백을 제거해줘야한다.

DB에 붙으려면 DataSource가  필요
dataSource에 DI 적용해주고
dataSource.getConnection() 해주면 실제 db와 연결되는 소켓을 얻게된다.

ResultSet : 결과를 받는거임

어떤 코드도 손 안대고 spring이 제공하는 configuration만 수정해서 수정할 수 있다. - 이게 스프링을 쓰는 이유

<h2> OCP(개방 폐쇄 원칙)</h2>
확장에는 열려있고, 수정, 변경에는 닫혀있다.
의존 관계가 있는 것들을 다 수정해줬어야했던 과거에비해서
의존 관계 주입을 이용하면 기존의 코드에는 손 하나도 안되고 config 설정 코드만 수정하면 구현 클래스를 변경할 수 있다. config에서는 코드들을 조립한다고 생각하면 된다.

# 스프링 통합 테스트
@SpringBootTest 스프링 컨테이너와 테스트를 함께 실행 한줄로 가능
@Transactional
	데이터베이스에 commit 하기 전에는 반영이 안되는데, 이 어노테이션이 달려있으면, 테스트 시작 전에 트랜잭션을 시작하고 끝난 후에 롤백해서 디비에 테스트 데이터가 남지않아서 다른 테스트에 영향을 주지않게된다. 지운다는 것보다는 반영을 안하는 것이다.
@Commit
	데이터베이스에 commit함

기존의 @BeforeEach에서 객체 생성해서 넣어줬던거를 컨테이너한테 달라고 요청해야함.

단위테스트를 잘 만들어서 테스트 해보는 것이 더 중요하다.
작은 버그 하나가 큰 피해를 줄 수 있기 때문에 테스트 코드를 잘 짜는 것이 중요하다. 테스트를 꼼꼼하게 작성하고 테스트케이스를 잘 만들어야한다. 


# 스프링  JdbcTemplate
반복코드의 대부분을 제거해줌
jdbcTemplate.query(쿼리문, 매퍼)
패턴써서 줄이고 줄인 것이다. 템플릿 메서드 패턴을 이용해서 줄임.

# 스프링 데이터 JPA
리포지토리에 구현 클래스 없이 인터페이스 만으로도 개발을 할 수 있다.
디테일한 부분은 좀 다르겠지만 CRUD 기능도 스프링 데이터 JAP가 제공함

스프링 데이터 JPA 가 인터페이스를 보고 구현체를 만들어서 가져온다.

제공 클래스

![page56image61015584](https://user-images.githubusercontent.com/16794320/103343957-9ba70780-4ad0-11eb-8cc0-872ef81bb998.png)

기본적인 crud와 단순조회가 다 제공된다.
페이징 기능까지 자동으로 제공한다.

복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 이용한다.
-자세한 내용은 ORM에서 자세한 설명할 예정

# AOP
공통 관심사항과 핵심 관심사항을 분리하는 것

언제 왜 쓰는지 알기
원하는 곳에 공통 관심 사항을 적용하고싶을 때 사용한다.
적용하고싶은 곳에 지정하면 지정된다.
config에 bean으로 등록해놓는게 좋다
	어떤게 등록돼서 사용되는지 볼 수 있기 때문이다.

@Aspect
	이게 달리면 AOP가 지정된다.
  
@Around(“Pointcut”)
	속성값으로 PointCut을 전달해 줘야함.
	Pointcut
		부가기능이 적용될 JoinPoint를정의한 것 
		execution(접근제어자, 반환형 패키지를 포함한 클래스경로 메소드 파라미터)
		자세한 내용은 문서를 찾아보면서 추가할 예정

스프링에 AOP가 있으면 컨테이너에 스프링 빈을 등록할 때 가짜 스프링 빈(목 오브젝트라 했던거로 기억)을 세워서 joinPoint.proceed()를 사용하면 그때 진짜를 호출해 줌. 
그래서 컨트롤러가 호출하는거는 프록시라는 기술로 발생하는 가짜 서비스다.
이런 것들을 할 수 있는 기반이 되는게 DI다.

# 단축키
command option n - inline으로 합칠 수 있다.

control + R - 직전의 수행한 테스트를 다시 실행.
