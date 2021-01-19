# 도메인 분석 설계
## 도메인 모델과 테이블 설계
가급적이면 양방향 연관관계보다는 단방향 연관관계

상속관계 매팅 중 가장 단순한 
싱글테이블 전략
한 테이블에 다 넣고 DTYPE으로 구분해서 사용하는 방식

1대다 관계에서는 다에 무조건 외래키가 존재하게된다.
외래키가 있는 쪽을 연관관계의 주인으로 정하는 것이 좋다.
(누가 관리할지 정하는 것이다)
이렇게하면 외래키에대한 업데이트가 자동으로 되면서 관리와 유지보수가 쉽다.


@DiscriminatorValue(“B”)
DB에 저장되는 벨류 설정

Enum쓸때는 반드시
@Enumerated(EnumType./STRING/) 써야한다.
ORDINAL 쓰면 중간에 환경이 바뀌거나하면 디비에 큰 타격을 줄 수 있다.

joinColumn을 사용하면 외래키를 설정한다.

1대1 관계일 때는 JPA에서는 외래키를 엑세스를 많이 하는 
곳에 연관관계의 주인을 준다.


## 엔티티 클래스 개발 
실무에서는 엔티티의 데이터를 조회하는 일이 정말 많기 때문에  Getter는 잘 열어둔다.
Setter를 열어두면 여기저기, 여러 서비스 메서드에서 호출해서 엔티티를 바꾸고 있으면, 추후에 변경을 해야할 때, 엔티티가 어디서 바뀌는지 Setter를 다 뒤지면서 찾아야하는 일이 벌어진다. 그래서 Setter 대신에 변경 지점이 명확하도록 변경을 위한 비즈니스 메서드를 별도로 제공한다.


### 코딩

관례상 엔티티의 식별자는 id를 사용하고, PK 칼럼명은 클래스명_id를 사용. 테이블은 타입이 없어서 구분이 없어서 테이블명_id
빨간 언더라인이 상당히 불편하지만 아랑곳하지않고 해야한다. 

클래스 만들면
@Entity
@Getter @Setter
를 해준다.
테이블인 경우에는 @Table(name = “테이블 이름”)
key value에는
@Id @GeneratedValue
붙여준다.

그리고 모든 값에는 
@Column(name= “컬럼이름”) 을 붙여준다.

@Embeded로 사용하는 애는
클래스 선언 후에 @Embeddable을 붙여준다. 그리고 값을 알아서 설정하도록 @Setter를 붙인다.

DB 매핑 방식도
@OneTo~ 인 애들은 cascade로 종속성 설정을 한다.
@OneToOne

@OneToMany
mappedBy를 설정해서 외래키 받아올 클래스 지정
@ManyToOne
fetch value 설정해주기
@ManyToMany
가능은 하지만 실무에서는 쓰지말자.절대로!
추가적인 매핑이 불가능하다. -> 필드를 더 추가하거나 하는게 불가능하다. 값을 더 못넣다보니 수정도 힘들고 운영하기도 너무 어렵다.

self로 양라인 연관관계를 걸어줄 수 있다.
클래스에 포함된 애들끼리도 연관관계를 걸어줄 수 있다.

값 타입은 생성할 때 값을 설정하고, @Setter를 안열어줘서 불변성을 유지하게 해준다.
JPA 스펙상 엔티티나 임베디드 타입(@Embeddable)은 기본 생성자를 pulic이나 protected로 해야하는데, 그래서 값 타입같은 경우는 생성자를 protected로 설정해줘야한다. 그렇게해야 JPA 구현 라이브러리가 객체를 생성할 때 리플랙션 같은 기술을 사용할 수 있다.

## 엔티티 설계시 주의점
### 엔티티에는 가급적 Setter를 사용하지말자. 
Setter를 열어두면 변경 포인트가 너무 많다. 그렇게되면 유지보수가 어렵다.

### 모든 관계는 지연로딩으로 설정해야한다.
즉시로딩은 예측이 어렵고, 어떤 SQL이 실행될지 추적하기가 어렵다.
그래서 LAZY로 설정해줘야한다. 안그러면 연관된 애들 전부 다 가져온다. n+1문제가 생긴다. 쿼리 하나 날렸는데, n개의 쿼리가 잔뜩 생산된다. 
XToOne 관계는 반드시 fetch를 LAZY로 설정해줘야한다.

### 테이블, 컬럼명 생성 전략
실제 DB에 반영되는 이름 규약과 비슷하게해준다.
카멜케이스 대신 언더스코어(_)를 사용한다.
.대신 _을 사용.
대문자는 소문자로 바꾼다.
그게 안되어있으면 스프링이 알아서 바꿔준다. 하지만 이 규약을 지켜서 이름 짓는게 좋다.

### Cascade
cascadeType.ALL로 해주면 관련된 애들 함께 persist하도록 할 수 있다.
원래 엔티티는 persist하면 각각 걸어줘야하는데 CascadeType을 이용해서 한번에 전파할 수 있다.

## 연관관계 편의 매서드
set을 해서 코드를 원자적으로 묶어주는 역할을 한다. 

## 단축키
맨날 적는다하고 까먹은 단축키 command + B
얘는 진짜 유용한 단축키다. 누르면 관련 위치로 이동한다.