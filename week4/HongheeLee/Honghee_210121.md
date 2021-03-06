# JPA 프로그래밍 - 기본편 2일차(영속성 관리)

## 영속성 관리

### 영속성 컨텍스트

JPA에서 가장 중요한 2가지 

- 객체와 관계형 데이터베이스 매핑(ORM) : 정적이고 설계 관련
- 영속성 컨텍스트 : 실제 JPA가 내부에서 어떻게 동작해?

영속성 컨텍스트 : 엔티티를 영구 저장하는 환경, JPA를 이해하는게 가장 중요한 용어, 논리적인 개념, 엔티티 매니저를 통해서 영속성 컨텍스트에 접근한다. 

**엔티티의 생명주기**

- 비영속 : 영속성 컨텍스트와 무관한 새로운 상태. 단순히 객체 생성만 한 상태

- 영속 : 영속성 컨텍스트에 관리되는 상태. em.persist(member)를 통해 바로 DB에 저장되는 건 아니고 영속성  컨텍스트에 저장. 

- 준영속 : 엔티티를 영속성 컨텍스트에서 분리한 상태. 

- 삭제 : 삭제된 상태

**영속성 컨텍스트의 이점**(어플리케이션과 DB 사이의 중간단계로써)

- 1차캐시 : @Id가 key가 되고 객체 자체가 value가 된다. em.find()하면 1차 캐시에서 먼저 조회하고 없으면 DB에서 조회. DB에서 찾으면 결과를 1차 캐시에 저장하고 반환. 한 트랙잭션 안에서만 공유됨.

- 동일성 보장 : 1차 캐시를 이용해서 같은 트랜잭션 단위에서는 영속 엔티티의 동일성 보장한다.

- 엔티티 등록(트랙잭션을 지원하는 쓰기 지연) : em.persist()한다고 쿼리를  DB에 보내지 않고 INSERT SQL 생성만 해서 영속 컨텍스트 안에 있는 쓰기 지연 SQL 저장소에 쌓아둔다. 커밋하는 순간 DB에 INSERT SQL을 보낸다. (commit -> flush - > commit)

- 변경 감지 : 찾아온 다음에 값 변경하고 다시 persist()나 update() 같이 DB에 따로 반영하는 코드 안써줘도 된다. 트랜잭션이 커밋되는 시점에 (flush -> 현재 엔티티와 스냅샷(처음에 1차캐시에 들어온 상태) 비교 -> UPDATE SQL 생성 -> flush -> commit), 엔티티 삭제도 마찬가지. 

**플러시** : 영속성 컨텍스트의 변경내용을 데이터베이스에 반영, 영속성 컨텍스트를 비우지는 않는다. 트랜잭션이라는 작업 단위 중요! -> 커밋 직전에만 동기화하면 됨 

영속성 컨텍스트를 플러시하는 방법 : em.flush(), 트랜잭션 커밋, JPQL 쿼리 실행

**준영속 상태** : 영속 상태 엔티티가 영속성 컨텍스트에서 분리(detached). 영속성 컨텍스트가 제공하는 기능 이용 X. 

준영속 상태로 만드는 방법 : em.detach(entity), em.clear(), em.close()
