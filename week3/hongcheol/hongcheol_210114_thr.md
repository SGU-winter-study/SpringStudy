스프링 부트가 타임리프 설정을 자동으로 해줘서 부르고싶은 html 파일 리턴하면 자동으로 연결해줘서 뷰를 보여준다.
resources:templates/ +{ViewName}+ .html

datasource 설정할 때, db url에
1.4.197 버전까지는 됨.
jpashop;MVCC=TRUE 붙여주는게 권장된다. - 1.4.198 버전부터는 안돌아감.

설정들은 spring boot에 들어가서 하나하나 공부하면서 파야한다.

sout으로 출력
-show_sql: true
log를 찍어줌
-org.hibernate.SQL: debug

실무 개발에서는 log를 찍어야하므로 아래것을 권장한다.

yml에서 :을 쓸 때 앞에 있는 애랑 붙여서 써야한다. 안그러면 작동을 안한다.
spring-boot-starter-data-jpa해주면서 entity manager 생성하는 팩토리같은 코드가 자동으로 생성되기때문에 그냥 가져다가 쓰면된다.

command랑 query를 최대한 분리하는 것이 좋다.

entitymanager에서 데이터를 변경하려면 반드시 트랜잭션 아래에서 작동해야한다.

영속성 컨텍스트 안에서 Id가 같으면 같은 엔티티로 식별한다. 

배포할 때는 jar로 배포한다.

쿼리 남기고 싶으면 yml 파일의 로그에 org.hibernate.type을 추가하거나 외부 라이브러리를 사용하면 된다. 개발 단계에서는 자유롭게 사용해도 좋지만, 운영단계에서는 테스트를 잘해서 검증해 사용해야한다.
