# 실전! 스프링 부트와 JPA 활용1 -  4일차(주문 검색 기능 ~ 웹 개층 개발)

### 주문 검색 기능 개발

JPA에서 동적 쿼리 어떻게 해결?

- JPQL 쿼리를 문자로  생성? 번거롭고 실수로 인한 버그 가능성 있다.

- JPA Criteria? 이건 JAP 표준 스펙이지만 너무 복잡하다. JPA Criteria만 보면 어떤 JPQL이 만들어지는지 그려지지 않고 유지 보수성이 매우 낮다.

- Querydsl? 동적 쿼리나 복잡한 정적 쿼리를 처리하는데  유용한 라이브러리. 

## 웹 계층 개발

### 홈화면과 레이아웃 

HomeController 등록 : 처음 들어가면 home.html를 렌더링 해준다.

타임리프 템플릿 등록(home.html) : replace 사용해서 다른 파일에서 header, footer 등을 반복해서 포함할 수 있다. 이 방식은 include style 인데 Hierarchical-style layouts으로도 만들 수 있다.

view 리소스는 부트스트랩 사용.

### 회원 등록

폼 객체를 사용해서 화면 계층과 서비스 계층을 명확하게 분리한다.

회원 컨트롤러에서는 @GetMapping으로 createForm() 만든다. 여기서 모델에다가 addAttribute해서 컨트롤러에서 뷰로 넘어갈 때 이 데이터 실어서 넘긴다. @PostMapping으로 html에서 넘어온 데이터들을 기반으로 Memberform을 만든다. 이때 스프링의 @Valid와 BindingResult를 통해 데이터를 검증할 수 있다. 

### 회원 목록 조회

조회한 상품을 뷰에 전달하기 위해 스프링 MVC가 제공하는 모델( Model ) 객체에 보관, 실행할 뷰 이름을 반환.(templates/반환값.html)

타임리프에서 ?를 사용하면 null 을 무시한다.

폼 객체 vs 엔티티 직접 : 요구사항 간단할 때는 등록이나 수정할 떄 엔티티 직접 써도 된다. 하지만 기능이 복잡해지면 엔티티에 화면 종속적인 기능이 계속 생기고 엔티티가 지저분해짐. 엔티티를 핵심 비즈니스 로직에만 의존하고 순수하게 유지해서 유지보수하기 쉽게 해야함.  API를 만들 때는 절대 엔티티를 반환하면 안된다.

### 상품 등록, 상품 목록

- 상품 등록 폼에서 데이터를 입력하고 Submit 버튼을 클릭하면 /items/new 를 POST 방식으로 요청(item 만들 때 setter로 필드값 지정하기 보다는 createBook()과 같이 정적 팩토리 매서드 패턴 사용)
- 상품 저장이 끝나면 상품 목록 화면( redirect:/items )으로 리다이렉트
- 상품 목록 : model 에 담아둔 상품 목록인 items 를 꺼내서 상품 정보를 출력

### 상품 수정

상품 수정 폼 이동

1. 수정 버튼을 선택하면 /items/{itemId}/edit URL을 GET 방식으로 요청
2. 그 결과로 updateItemForm() 메서드를 실행하는데 이 메서드는 itemService.findOne(itemId) 를 호출해서 수정할 상품을 조회
3. 조회 결과를 모델 객체에 담아서 뷰( items/updateItemForm )에 전달

상품 수정 실행

1. 상품 수정 폼에서 정보를 수정하고 Submit 버튼을 선택

2. /items/{itemId}/edit URL을 POST 방식으로 요청하고 updateItem() 메서드를 실행

3. 이때 컨트롤러에 파라미터로 넘어온 item 엔티티 인스턴스는 현재 준영속 상태다. 따라서 영속성 컨텍스트의 지원을 받을 수 없고 데이터를 수정해도 변경 감지 기능은 동작X

### 변경 감지와 병합(중요!!)

JPA가 관리하는 영속 상태 엔티티는 변경 감지해서 커밋 시점에 DB에 쿼리 날려준다. 

하지만 준영속상태 엔티티(영속성 컨텍스트가 더는 관리하지 않는 엔티티)는 변경 감지 하지 못한다. 어떻게 데이터 수정?!

변경 감지 기능 사용 / 병합(merge) 사용

- 변경 감지 기능 사용 : 영속성 컨텍스트에서 엔티티를 다시 조회한 후에 데이터를 수정하는 방법트랜잭션 안에서 엔티티를 다시 조회, 변경할 값 선택 -> 트랜잭션 커밋 시점에 변경 감지(Dirty Checking)이 동작해서 DB에 UPDATE SQL 실행

- 병합 사용 : 준영속 상태의 엔티티를 영속 상태로 변경할 때 사용한다.

  동작 방식

  1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회한다.
  2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체한다.(병합한다.)
  3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에 UPDATE SQL이 실행

변경 감지 기능은 원하는 속성만 선택해서 변경할 수 있지만 merge()는 모든 속성이 변경된다.  그러니 엔티티를 변경할 떄는 불편하더라도 항상 **변경 감지**로

### 주문 등록

- 주문 폼 이동
  - 메인 화면에서 상품 주문을 선택하면 /order 를 GET 방식으로 호출
  - 주문 화면에는 주문할 고객정보와 상품 정보가 필요하므로 model 객체에 담아서 뷰에 넘겨줌
- 주문 실행
  - 뷰에서 주문할 회원과 상품 그리고 수량을 선택해서 Submit 버튼을 누르면 /order URL을 POST 방식으로 호출
  - 컨트롤러의 order() 메서드를 실행. 이 메서드는 고객 식별자( memberId ), 주문할 상품 식별자( itemId ), 수량( count ) 정보를 받아서 주문 서비스에 주문을 요청
  - 주문이 끝나면 상품 주문 내역이 있는 /orders URL로 리다이렉트(return)

컨트롤러에서 식별자만 넘겨주고 서비스 계층에서 엔티티 조회부터 주요 비즈니스 로직 수행하는게 좋다. 그렇게 할 때 엔티티가 영속 상태로 남아있어서 값을 변경하는 등의 작업이 더 용이하다.

### 주문 목록 검색, 취소

주문 목록 검색은 주문 서비스의 findOrders() 메서드로 모든 주문을 리스트 형태로 가져온 뒤 model 객체에 담아서 뷰에 넘겨준다.

@ModelAttribute("orderSearch")로 파라미터 넘기면 모델에 이건 자동적으로 들어간다. addAttribute 안해도

주문 취소는 주문 서비스의 cancelOrder() 메서드를 활용한다.
