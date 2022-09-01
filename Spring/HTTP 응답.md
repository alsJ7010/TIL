# HTTP 응답

* **String 을 반환하는 경우 - view or HTTP message**
* `@ResponseBody`가 없으면 뷰 리졸버가 실행되서 뷰를 찾아 렌더링
* `@ResponseBody`가 있으면 뷰 리졸버를 싱행하지 않고, HTTP 메세지 바디에 직접 문자를 넣어 보낸다.

**HTTP 메시지**
* `@ResponseBody , HttpEntity` 를 사용하면, 뷰 템플릿을 사용하는 것이 아니라, **HTTP 메시지 바디에
직접 응답 데이터를 출력**할 수 있다.'

*  `@RestController` 애노테이션 안에는 `@Controller`와 `@ResponseBody` 가 담겨있다.

--------------------------------

Thymeleaf 스프링 부트 설정
타임리프 라이브러리를 추가하면 `application.properties`에 해당 경로가 기본으로 설정된다. 필요시에 수정
```
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

### HTTP응답을 보내는 방법 ###

1) response서블릿 사용, response에 직접 넣어주기    
 `public void responseBodyV1(HttpServletResponse response) throws IOException { ... }`

2) HTTPEntity 사용(텍스트/객체)    
```
public ResponseEntity<String> responseBodyV2() { 
    return new ResponseEntity<>("ok", HttpStatus.OK);
}
```    
```
public ResponseEntity<HelloData> responseBodyJsonV1() { 
    return new ResponseEntity<>(helloData, HttpStatus.OK);
}
```

3) **`@ResponseBody` 사용 > response에 넣어보내는것이 아니라 HTTP 메세지 바디에 직접 넣어 보내기.**   
  * String 또는 객체 모두 리턴 가능
