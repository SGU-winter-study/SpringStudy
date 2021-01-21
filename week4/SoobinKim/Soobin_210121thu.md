JPA(Java Persistence API)
데이터베이스에 접근할 때 개발자가 SQL문을 사용할 필요가 없어짐.

관계형 DB를 자주 씀. 그러나 여기에 객체지향 언어를 사용하게 되면 객체지향 언어의 장점을 전혀 사용할 수 없게 됨. (ex. 객체가 수정됨과 동시에 SQL문법에서도 전부 같이 수정해주어야 함. -> 실수 발생 확률 높아짐)

DAO에서: 동일 ID를 가진 객체라도 서로 다르다(!=)
자바 컬렉션에서: 동일 ID를 가진 객체라면 서로 같다(==)

객제지향의 특성을 살릴수록 관계형 DB로의 매핑 작업이 너무 번거로워진다.

ORM (Object-relational mapping, 객체 관계 매핑)
객체는 객체대로, 관계형 DB는 관계형 DB대로 설계

-> 해결책: JPA

* 생산성
C: jpa.persist(member)
R: Member member = jpa.find(memberId)
U: member.setName("newName")
D: jpa.remove(member)

* 유지보수
객체만 수정하면 되고, 쿼리를 수정할 필요가 없음.

* JPA와 패러다임의 불일치 해결

* JPA와 연관관계 저장, 객체 그래프 탐색

* 신뢰할 수 있는 엔티티, 계층
'지연 로딩'이라는 기능을 통해서 객체간의 상속관계에 이해 계층 구조에 의한 객체 그래프 탐색이 가능하다.

* 동일한 트랜젝션에서 조회한 엔티티는 같음(==)을 보장

* JPA의 성능 최적화 기능
	- 1차 캐시와 동일성(identity) 보장: 캐싱을 하고 있음
	- 트랜잭션을 지원하는 쓰기 지연(transactional write-behind): 버퍼링
	- 지연 로딩(lazy loading)
		- 지연 로딩: 객체가 실제로 사용되는 시점에 로딩
		- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회



``` java
<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
```
현재 어떤 DB 쿼리언어를 사용하고 있는지를 나타냄. 이를 통해 JPA가 나중에 db가 바뀌더라도 적절하게 일정 수준 이상 번역해서 사용 가능.

JPA를 사용하는 클래스임을 JPA로 하여금 인식하도록 하기 위해 JPA를 사용하는 class 위에 @Entity annotation을 꼭 붙여주어야 한다.

JPA는 기본적으로 해당 클래스명과 동일한 테이블의, 해당 필드 명과 동일한 컬럼에 데이터를 저장한다. 만약 다르게 직접 지정하고 싶다면, 클래스 선언 위에(@Entity 어노테이션 다음에) @Table = (name = "USER"), 해당 필드 위에 @Column = (name = "username") 과 같이 지정해주면 된다.

JPA는 마치 자바 컬렉션을 다루는 것처럼 설계되어 있기 때문에, 데이터를 수정할 때, 즉 EntityManager에서 특정 데이터를 찾아서 해당 데이터를 수정했을 때 다시 persist를 통해서 저장하는 과정이 필요하지 않다.

EntityManagerFactory 는 서버가 올라오는 시점에 딱 한 번만 생성되어야 한다. EntityManager는 고객의 요청이 올 때마다 썼다가 버렸다가 여러 번 생성된다. (오히려 EntityManager는 thread간 공유되면 안 된다.)

JPA는 절대 테이블을 대상으로 쿼리를 짜지 않는다. '객체'를 대상으로 함.
