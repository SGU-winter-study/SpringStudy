# 실전! 스프링 부트와 JPA 활용1 -  1일차(프로젝트 환경설정)

## 프로젝트 환경설정

### 프로젝트 생성

프로젝트 생성은 스프링 부트 스타터를 통해서 간단히 할 수 있다. 

스프링 부트 스타터 페이지에서 gradle project 선택하고 web, thymeleaf, jpa, h2, lombok, validation를 dependencies로 선택한다.

그 결과로 만들어진 프로젝트를 다운로드 받아 Intellij에서 build.gradle 파일을 프로젝트로 열면 프로젝트 생성이 완료된다. 

### View 환경설정

thymeleaf 템플릿엔진을 사용한다.

컨트롤러가 model에 데이터 넣어서 넘겨주면 thymeleaf가 templates 아래 있는 html에 데이터를 넣어서 렌더링 해줄 수 있다. 

### H2 데이터베이스

H2 데이터베이스 메모리 모드로도 쓸 수 있고 개발이나 테스트 용도로 편하다. 

H2 설치한 이후에 윈도우에서는 H2/bin/h2.bat을 실행하면 데이터베이스에 접근할 수 있다.

jdbc:h2:~/jpashop를 입력하면 db 파일을 만들 수 있고 

그 다음부터는 jdbc:h2:tcp://localhost/~/jpashop로 접근한다.

### JPA와 DB 설정, 동작 확인

속성 많아지면 properties 파일보다 application.yml 파일이 편하다.

코드 작성할 때는 커맨드와 쿼리를 분리해라. 예를 들면 save하고서 member 자체를 리턴하는 것보단 id정도만 리턴하도록.

테스트에 @Transactional 붙으면 테스트 끝난 뒤에 롤백해서 DB에는 데이터 남지 않는다. 직접 확인하고 싶으면 @Rollback(false) 넣는다.

영속성 컨텍스트 안에서 식별자(Id)가 같으면 같은 엔티티로 식별한다. 그래서 member를 save하고 find하면, member가 영속성 컨텍스트 안에 있으니 1차 캐시에서 꺼내오게 된다.

쿼리 남기고 싶으면 yml 파일의 로그에 org.hibernate.type 추가하거나 외부 라이브러리 사용하면 된다!
