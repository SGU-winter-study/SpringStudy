# 상품 도메인 개발
데이터를 가지고있는 쪽에서 비즈니스 메서드가 있는게 좋다.

서비스는 리포지토리에 단순하게 위임만하는 클래스다.

여러개 찾는 경우에는 jpql을 작성해야함을 잊지말자.

# 주문, 주문상품 엔티티 개발
로직별로 나눠서 개발하는 것은 필수.

new에서 set set set 하는게 아니라 생성할 때, create를 호출해서 생성매서드에서 완성할 수 있게해서 한 곳에 응집해놔야한다. 그래야 수정할 때, 해당 부분만 가서 고치는 것으로 전체 시스템을 수정할 수 있다.

클래스를 먼저 만들고 구현해야할 것을 적고 개발하면 뭘할지 명확하게 보이는 것 같다.

이번에도 역시 서비스는 엔티티에 필요한 요청을 위임하는 역할을 한다. 현재 개발 단계는 도메인 모델 패턴을 적용했기때문에 서비스에서는 비즈니스 로직이 거의 없다. 개발 과정에서 도메인 모델 패턴과 트랜잭션 스크립트 패턴은 양립할 수 있고, 개발환경에서 유지보수가 쉬운 쪽을 선택해서 사용하면 된다.

엔티티가 비즈니스 로직을 가지고 객체 지 향의 특성을 적극 활용하는 것을 도메인 모델 패턴(http://martinfowler.com/eaaCatalog/ domainModel.html)이라 한다. 반대로 엔티티에는 비즈니스 로직이 거의 없고 서비스 계층에서 대부분 의 비즈니스 로직을 처리하는 것을 트랜잭션 스크립트 패턴(http://martinfowler.com/eaaCatalog/ transactionScript.html)이라 한다. 



cascade의 범위는 참조하는게 주인이 private owner 일때만 써야한다.
다른 곳에서 안쓰고 오직 한 곳에서만 참조하는 애,즉 라이프 사이클에대해서 동일하게 관리할 때, 사용하면 도움을 받을 수 있다. 다른 곳에서 참조하고 가져다 쓰게되면 cascade를 막 쓰면, 하나 지웠는데 다 지워지거나 하는 경우가 있다. 그럴 때는 별도의 리포지토리를 생성해서 persist해서 사용하는 것이 좋다.

생성자를 protected로 설정해서 비즈니스 로직에서 create 매서드를 호출해서 사용하는 방식이 아닌 해당 클래스에서 new로 해서 값을 채워넣는 방식으로 개발하는 경우에 빨간 줄을 긋거나 에러를 발생시키는 것이 좋다. 이렇게하면 좋은 설계와 유지보수를 얻을 수 있다. 

 # 테스트

좋은 테스트는 db에 의존하지않고 그 매서드 자체에대해서만 검증하는 테스트다.

assertEquals(message, expected, actual);


동적쿼리를 해결하는 방법

동적으로 생성해줘야한다.
그때 직접 짜는거보다는 query메서드를 이용해서 사용하는 것이 좋다. 그래야 컴파일 시점에 오류를 잡아낼 수 있다.

# 웹 계층 개발
본격적인 MVC개발
Homecontroller 등록을 해주고, 타임리프 템플릿을 등록해서 사용해준다. 이따 include style과 hierarchical로도 만들 수 있다.

회원등록은 폼 객체를 따로 생성해줘서 원하는 양식으로 회원정보를 입력받아서 처리할 수 있다. @GetMapping으로 폼을 보여줄 url을 설정해주고 createForm()을 만들어준다. 

API를 만들 때는 엔티티를 API로 외부 노출을시키면 안된다.. 그렇게되면 엔티티에 로직을 추가할 때, API가 수정되는 경우가 생겨 상당히 불안해진다.

등록이나 읽어오는건 쉽니다.
컨트롤러에 파라미터로 넘긴 item 엔티티 인스턴스는 준영속상태로 넘어간다. 그래서 영속성 컨텍스트에대한 지원을 받을 수 없으며, 데이터를 수정한다해도 변경을 감지하는 등의 기능을 하지 못한다.

## 변경 감지와 병합
### 준영속 엔티티란?
영속성 컨텍스트가 더이상 관리하지않는 엔티티다.

### 준영속 엔티티를 수정하는 방법
#### 변경감지 기능 사용
영속성 컨텍스트에서 다시 엔티티를 조회한 다음에, 데이터를 수정하는 방법이다. 
#### 병합 사용
준영속인 애를 영속으로 바꿀 때 사용하는 기능이다.

#### 병합시 동작 방식
1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회한다. 

2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체한다.(병합한다.) 

3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에 UPDATE SQL이 실행 

> 주의: 변경 감지 기능을 사용하면 원하는 속성만 선택해서 변경할 수 있지만, 병합을 사용하면 모든 속성이 변경된다. 병합시 값이 없으면 null 로 업데이트 할 위험도 있다. (병합은 모든 필드를 교체한다.) 

가장 좋은 방법은 엔티티를 변경할 때는 항상 변경 감지를 이용하는 것이다. 또한 컨트롤러에서 엔티티를 임의로 생성하지말아야한다.


# 단축키
command option p를 해주면 파라미터 추출 가능
command shift T 테스트와 대상을 옮겨다닐 수 있다.


