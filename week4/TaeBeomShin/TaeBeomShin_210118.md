<h1>스프링 부트와 JPA 활용1 - 3일차(회원 도메인 개발) </h1>

개발은 총 4부분으로 나누어서 진행한다.
도메인 개발, 웹계층 개발(Thymeleaf), API, 성능최적화의 4단계로 나누어 진다.
활용 1편에서는 도메인 개발과 웹계층 개발 부분을 다루고, 2편에서 API와 성능 최적화를 다룬다.

## 회원 도메인 개발
- 구현 기능 : save(), findAll(), findByName(), findOne()
- 이전에 기초 원리 강의에서 했던 내용 + Jpa(jpql, 새로운 annotation)인 정도의 내용.

* jpql vs sql 
: jpql은 객체대상의 쿼리라면 sql은 테이블을 대상으로 한 쿼리이다.
    (jpql로 작성한 쿼리는 jpa에서 sql로 바뀌어진다.)

* 기술 설명(Annotation)
- @Repository : 스프링 빈으로 등록.
- @PersistenceContext : EntityManeger 주입
- @PersistanceUnit : 엔티티 매니터 팩토리 주입.

## 회원 서비스 개발
- 구현 기능 : join(), findMember(), findOne()

* 기술 설명
- @Service : 스프링 빈으로 등록.
- @Transactional, readOnly=true -> 데이터의 변경이 없는 읽기전용 메서드에 사용.
- @Autowired : 생성자 Injection(생성자 만들지 않고 생성자 만든 것처럼 주입.)

## 회원 기능 테스트
- given, when, then 테스트 전략 : 주어진 상황에서 실행했을때 기대되는 결과.
- 테스트 시, 테스트는 격리된 환경에서 실행하고, 끝나면 데이터를 초기화 하는 것이 좋으므로 메모리 DB를 이용한다.
- test/resource에 application.yml을 추가함으로써 이런 설정을 마칠 수 있다.

## 상품 엔티티, 레포지토리, 서비스

* 비즈니스 로직은 항상 접근하려는 데이터가 있는 클래스에 구현하는 것이 좋다.
