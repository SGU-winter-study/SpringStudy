## MVC 를 통한 데이터 관리

```HTML
<input type="text" id="name" name="name" placeholder="이름을 입력하세요">
```
여기서 name="**name**" 을 통해 도메인의 Member 객체 속 **name** 이라는 변수에 사용자가 입력한 값(text)이 저장되게 된다.
이러한 작업을 처리하는 곳이 Controller이다. Controller의 어떤 메소드에 의해 이 작업을 처리할 것인지를 명시하기 위해
```HTML
<form action="/members/new" method="post">
```
라고 html 파일에 form 태그를 생성하고,
Controller에
```Java
@PostMapping("members/new")
    public String create(MemberForm form) {
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/";
    }
```
이와 같이 정의한다.
**@PostMapping** 이라는 annotation을 통해 해당 함수가 post방식으로 데이터를 전송하는 form 태그를 관리하기 위한 메소드임을 알 수 있고, "members/new"라는 action과 매핑된다는 것을 표시한다.

- - -

## thymeleaf
회원 목록을 나타내는 페이지의 html 코드는 다음과 같다.
```HTML
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
	<div>
		<table>
			<thead>
			<tr>
				<th>#</th>
				<th>이름</th>
			</tr>
			</thead>
			<tbody>
			<tr th:each="member : ${members}">
				<td th:text="${member.id}"></td>
				<td th:text="${member.name}"></td>
			</tr>
			</tbody>
		</table>
	</div>
</div> <!-- /container -->
</body>
</html>
```
여기에서 중간중간 보이는 **th** 라는 것이 thymeleaf 문법임을 나타내는 지표이다.
* th:each 는 java의 for each 구문과 유사한 역할을 수행한다. 해당 attribute name을 갖는 객체를 차례대로 꺼낸다.
ex) ```<tr th:each="member : ${members}">```
'members'에 들어 있는 데이터들에 대하여 loop 를 수행한다.
이때 'members' 라는 변수는 Controller 에서 해당 View html 파일로 보내주어야만 인식할 수 있다.
(실제로 Controller에서 model에 addAttribute시 attributeName으로 "members"를 지정함을 확인할 수 있다.)

* th:text 를 통해 해당 tag 사이에 text 값을 삽입할 수 있다.
ex) ``` <td th:text="${member.id}"></td> ```
th:each 를 통해 "members" 에서 꺼낸 객체가 "member"에 저장되게 되고, 
이 "member" 객체 내부의 "id" 값이 text 형태로 <td> tag 사이에 표시되게 된다.
이를 통해 코드를 중복하여 사용하지 않고도 html table 을 생성할 수 있다.
		
- - -		
## 스프링 DB
여태까지 생성한 웹 페이지는 스프링 내부 메모리를 사용하고 있었기 때문에 서버를 껐다 켜면 모든 값이 초기화된다.
이를 방지하기 위하여 별도의 파일(ex. 메모장) 또는 외부 DB를 사용해야 한다.

* H2 데이터베이스
: 오픈소스 JDBC API
* 순수 JDBC
* Spring JdbcTemplate















