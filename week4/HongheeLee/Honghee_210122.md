# JPA 프로그래밍 - 기본편 3일차(엔티티 매핑)

## 엔티티 매핑

- 객체와 테이블 매핑 : @Entity, @Table

- 필드와 컬럼 매핑 : @Column

- 기본 키 매핑 : @Id

- 연관관계 매핑 : @ManyToOne, @JoinColums

### 객체와 테이블 매핑

- @Entity

  - @Entity가 붙은 클래스는 JPA가 관리하고 엔티티라고 한다. JPA를 사용해 테이블과 매핑할 클래스에 붙임. 

  - **기본 생성자 필수**
  - final 클래스, enum, interface, inner 클래스 사용 X
  - 속성엔 name.  JPA에서 사용할 엔티티 이름 지정, 기본값은 클래스 이름 그대로.

- @Table은 엔티티와 매핑할 테이블 지정. name 속성으로 매핑할 테이블 이름 지정가능. 기본값으로는 엔티티 이름 사용.

### 데이터베이스 스키마 자동 생성

DB 방언을 활용해서 DB에 맞는 DDL을 애플리케이션 실행 시점에 자동 생성. 객체 중심

hibernate.hbm2ddl.auto 속성 value = 

- create(DROP+CREATE)
- create-drop(DROP+CREATE+DROP)
- update(변경분만 반영, alter),
- validate(엔티티와 테이블이 정상 매핑되었는지 확인) 
- none : 사용

운영 장비에는 절대 create, create-drop, update 사용하면 안된다. 개발 초기에만 create, update 사용하고 테스트 서버나 운영에서는 validate, none 사용하자! 자동 생성된 DDL 보고 직접 수정해서 DB에 입력하자.

### 필드와 컬럼 매핑

매핑 어노테이션

- **@Column** : 컬럼 매핑
  -  name : DB 테이블의 컬럼명 따로 지정.
  - nullable : false하면 DDL 생성시 not null 제약조건 붙음
  - insertable, updateable : 등록 변경 가능여부
- @Temporal: 날짜타입 매핑. TemporalType. DATE, TIME, TIMESTAMP 선택. 최근 버전 사용하면 LocalDate, LocalDateTime 그냥 사용하면 된다.
- @Enumerated : enum 타입 매핑. EnumType.STRING으로 지정해서 enum 이름을 DB에 저장. ORDINAL 사용X
- @Lob : Lob은 대형 데이터를 저장하는 타입. BLOB(binary), CLOB(character) 매핑. 속성  X
- @Transient : 특정 필드를 컬럼에 매핑하지 않음. 이 필드는 메모리에서만 사용

### 기본 키 매핑

@Id : 직접 할당

@GeneratedValue : 자동 생성. 전략 4가지 중 선택 가능

- IDENTITY : 데이터베이스에 위임, MYSQL. 

  영속성 컨텍스트에서 PK값 없으면 안되는데 이 전략으로 하면 DB에 들어가야 PK값 정해지기 때문에 예외적으로 em.persist() 시점에 즉시 INSERT SQL 실행하고 DB에서 식별자를 조회.

- SEQUENCE : 데이테베이스 시퀀스 오브젝트 사용. ORACLE. @SequenceGenerator 필요. em.persist() 할 때 PK 있어야 하는데 seqnece 전략이면 쿼리 보내서 DB에서 먼저 seq의 다음값 얻고 객체에 PK값 넣어준다. INSERT SQL은 트랜잭션 커밋시점에 나간다. 버퍼링 가능. 

  allocationSize : 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용)

- TABLE : 키 생성용 테이블 사용해서 데이터베이스 시퀀스 흉내냄. 모든 DB에 적용가능. 성능떨어짐. 

- AUTO: 방언에 따라 자동 지정, 기본값

권장하는 식별자 전략

DB의 기본 키 제약 조건 : NULL 아니고, 유일, **변하면 안됨** 

미래까지 이 조건을 만족하는 자연키는 찾기 어렵다. 대리키를 사용하자!!  

**권장 : Long형 + 대체키 + 키 생성전략 사용**

### 예제 1 - 요구사항 분석과 기본 매핑

데이터 중심 설계의 문제점

- 현재 방식은 객체 설계를 테이블 설계에 맞춘 방식, 테이블의 외래키를 객체에 그대로 가져옴

- 객체 그래프 탐색 불가능, 참조가 없으므로 UML도 잘못됨
