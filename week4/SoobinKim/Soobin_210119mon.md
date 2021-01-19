@SpringBootApplication
해당 위치부터 설정을 읽어나가며 스프링 부트 자동 설정 세팅
-> 항상 프로젝트의 최상단에 위치해야 함

@RestController
컨트롤러를 JSON을 반환하는 컨트롤러로 만들어준다.

Dto = Data Transfer Object
데이터를 전송하기 위한 객체. 데이터를 담기 위한 private 변수와 해당 변수 설정을 위한 Getter, Setter로 이루어져 있다.
```java
import lombok.Getter;
import lombok.RequiredArgsConstructor;

@Getter
@RequiredArgsConstructor
public class HelloResponseDto {

    private final String name;
    private final int amount;

}
```

ORM = Object Relational Mapping
( != SQL Mapper)
자바 표준 ORM = JPA

