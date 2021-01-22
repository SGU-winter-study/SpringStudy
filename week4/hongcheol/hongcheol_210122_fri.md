# JPA 프로젝트 생성
persistence.xml 파일은 반드시 프로젝트 폴더의 src/main/resources/META-INF 디렉터리에 넣어줘야한다. 여기가 표준 위치다.

## persistence-unit name으로 이름 지정 
-사용하려는 이름을 설정한다.

Persistence./createEntityManagerFactory/(“사용하려는 이름”);
그리고 이런 식으로 가져다가 쓰면 된다.

## javax.persistence로 시작
 JPA 표준 속성이다.  
### JPA 구동 방식
persistence 클래스에서 설정정보를 저장해놨다가 설정 정보를 읽어서 EntityManagerFactory를 만들어서 필요할 때마다 EntityManager를 만들어내서 관리하는 방식으로 사용한다.


## hibernate로 시작: 
하이버네이트 전용 속성 

## 데이터베이스 dialect
JPA는 특정 데이터베이스에 종속되면 안된다.

각각의 데이터 베이스가 제공하는 SQL 문법과 함수가 조금씩 다른데, 누군가는 이걸 맞춰줄 필요가 있다. 그런 설정을 JPA 설정을 할 때
<property name=“hibernate.dialect” value=“org.hibernate.dialect.H2Dialect”/>

와 같이 써주면 이런 방언을 사용해서 개발해~ 라고 말해주면된다. 그러면 JPA가 알아서 해석하고 사용한다.


## 개발
@Entity를 붙여서 관리해야하는 애임을 알려준다.
엔티티 매니저 팩토리는 실행시점에 딱 하나만 만들어줘야한다. 

트랜잭션 단위로 디비 커넥션을 얻어서 쿼리를 날리고 할때마다 엔티티 매니저라는 애를 생성해서 사용해줘야한다.
엔티티 메니저는 쓰레드간에 공유를 하면 큰일난다. 사용하고 close해줘서 버려야한다.

JPA에서는 트랜잭션이 정말 중요하다.
모든 데이터를 변경하는 과정은 트랜잭션 안에서 관리되어야한다.

try - catch를 이용해서 관리해준다.
try해서 정상 작동하면 commit을 하게하고 오류가 발생한 경우에는 rollback()을 하고,
마무리로 em을 close()해줘야한다.
em은 자바 컬랙션처럼 객체를 대신 저장해주는 녀석이라고 생각하자.

멤버 저장방법
em.persist(대상)

수정은 JPA의 특징을 이용한다.
JPA를 통해서 가져온 애는 JPA가 관리를 한다. 그리고 JPA가 이게 변경됐는지 안됐는지를 commit할 때 확인해서, 바뀌었으면 commit하는 시점에 UPDATE 쿼리를 날려서 변경을 해준다.


데이터를 변경한 다음에
EntityTransaction에서 commit을 해줘야지만 쿼리가 나가서 디비에 저장된다.

이때, 클래스에 @Table(name = “테이블 이름”)을 해주면, 디비에 있는 테이블을 지정해서 쿼리를 쏴줄 수 있다.


## JPQL
통계형 쿼리나 다른 쿼리 등을 날릴 필요가 있을 때, JPQL을 날려준다.

객체를 상대로 쿼리를 날리는거라고 생각한다.
엔티티 객체를 중심으로 개발하는 JPA 생태계에서 검색 쿼리를 해야할 때, 테이블이 아닌 엔티티 객체를 대상으로 검색을 할 수 있게 해준다. 

애플리케이션이 필요한 데이터만 DB에서 불러오기 위해서는 결국 검색 조건을 포함한 SQL을 날려줘야하는데, JPQL을 이용해서 날려준다.

pagenation 하는데 유용하다.
