# JPA 프로그래밍 - 기본편 1일차(강좌 소개 ~ JPA 시작하기)

## JPA 소개

### SQL 중심적인 개발의 문제점

지금 시대는 객체를 관계형 DB에 관리 -> 결국 SQL 중심적인 개발이 됨 -> 무한 반복, 지루한 코드

 객체 vs 관계형 데이터 베이스 : 패러다임의 불일치 + 상속, 연관관계, 데이터 타입, 데이터 식별 방법에서도 차이가 있음.

계층형 아키텍처에서 진정한 의미의 계층 분할이 어렵다. 물리적으로는 나눠져 있지만 논리적으로는 강하게 결합되어 있음. 

객체답게(객체지향적으로) 모델링 할수록 매핑작업만 늘어난다. 그러다보면 SQL에 맞게 설계하게 됨.이를 해결하기 위해 객체를 자바 컬렉션에 저장하듯이 DB에 저장하는 방법? **JPA**

###  JPA란?

JPA : Java Persistence API, 자바 진영의 ORM 기술 표준

ORM : Object-relational mapping(객체 관계 매핑), 객체는 객체대로, RDB는 RDB대로 설계한 후 ORM 프레임워크가 중간에서 매핑.

JPA는 애플리케이션과 JDBC 사이에서 동작. 저장, 조회 등의 과정에서 알아서 분석해서 쿼리 생성해주고 JDBC API를 사용해서 결국 패러다임의 불일치를 해결해준다.

JPA 왜 사용해야 하나? 객체 중심으로 개발, 생산성, 유지보수, 패러다임 불일치 해결(상속, 연관관계, 객체 그래프 탐색, 비교), 성능 최적화(지연로딩, 즉시로딩)

결국 JPA를 활용하면 자바 컬렉션에서 했던 것처럼 RDB를 사용할 수 있게 해준다. 하지만 ORM은 객체와 RDB를 매핑해주는 역할이기 때문에 둘다 잘 알아야한다. 

## JPA 시작하기

### 프로젝트 생성

H2 데이터베이스 사용. 메이븐으로 자바 라이브러리, 빌드 관리함.

라이브러리 추가는 pom.xml에 JPA 설정은 persistence.xml에 해준다.

JPA는 특정 DB에 종속되지 않고 Dialect를 사용해 사용하는 DB에 맞게 SQL을 생성해준다.

###  애플리케이션 개발

JPA가 관리할 객체 -> @Entity, 데이터베이스 PK와 매핑 -> @Id

EntityMangerFactory는 하나만 생성해서 애플리케이션 전체에서 공유, EntityManger는 쓰레드간 공유 X

JPA에서는 트랜잭션이 중요! 데이터 변경은 트랜잭션 안에서만 일어나야 한다. 변경했으면 tx.commit()으로 커밋해야 반영되고, 다썼으면 em.close()해서 반환해야한다.

JPQL은 엔티티 객체를 대상으로 하는 객체지향쿼리 언어.  SQL은 데이터베이스 테이블을 대상으로 쿼리
