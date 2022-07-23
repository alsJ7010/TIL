# HTTP 요청 파라미터 받기



### HTTP 요청 파라미터 - @RequestParam 를 받을 때 ###

**1) request 서블릿으로 받는 방법**
`public void requestParamV1(HttpServletRequest request, HttpServletResponse response) throws IOException { ... }`

**2) @RequestParam으로 받는 방법1**
`public String requestParamV2(
            @RequestParam("username") String memberName,
            @RequestParam("age") int memberAge) { ... }
`

**3) 2) @RequestParam으로 받는 방법2 _ @RequstParam 일부 생략가능. 단 변수 명과 요청 파라미터 키 값이 같을 때**
`public String requestParamV2(
            @RequestParam String memberName,
            @RequestParam int memberAge) { ... }
`

**4) 단순 타입이면 @RequestParam 전체 생략 가능. 단 변수 명과 요청 파라미터 키 값이 같을 때**
`
public String requestParamV4(String username, int age) { ... }
`
하지만 모든 생략보다는 직관적으로 볼 수 있는 코드가 좋을수도 있음..



**5) 모든 요청 파라미터를 다 받고싶은 경우! -> map 으로 받을 수 있음**
`public String requestParamMap(@RequestParam Map<String, Object> paramMap) { ... }`
요청 파라미터의 같은 키 값이 여러개인 경우 -> `MultiValueMap`  



**그 밖에...**

* 해당 파라미터로 요청이 안들어온 경우 -> null 이 들어옴
* 요청 파라미터는 있는데 값이 없는 경우 -> ""빈값
* @RequestParam 파라미터 필수 여부 : `(required=true)` 
  * 디폴트값은 true
* @RequestParam 파라미터 디폴트 값 : `(defaultValue="")`
   * 빈값으로 들어온 경우에도 디폴트 값이 적용됨

----------------------------------

### HTTP 요청 파라미터 - @ModelAttribut ###

객체를 만들고 그 객체에 요청 값을 넣을 때
**1) 객체를 만들고 그 객체에 요청 값을 넣을 때**
`public String modelAttributeV1(@ModelAttribute HelloData helloData) { ... }`

혹은
`public String modelAttributeV1(HelloData helloData) { ... }`
@ModelAttribute도 생략 가능  
`@ModelAttribute` 가 있으면 스프링 MVC가 해당 타입의 객체를 생성한 후 객체 프로퍼티를 찾아 값을 바인딩 한다.

-------------------------------

스프링에서 컨트롤러에서 String 타입의 메소드에 String값을 리턴하면 **기본적으로 viewResolver에 의해 반환값에 맞는 뷰 이름을 찾게 된다**.  
**단순한 String값을 반환하기 위해서는**

1) 클래스 레벨에 @Controller 대신 `@RestController`를 써주거나
2) 메소드 레벨에 `@ResponseBody` 애노테이션을 붙여준다.

그러면 뷰 이름을 찾지 않고 리턴값 텍스트 그대로 HTTP message body에 넣어서 반환하게 된다.


                                                
**참고**
> 스프링은 해당 생략시 다음과 같은 규칙을 적용한다.
> * String , int , Integer 같은 단순 타입 = @RequestParam
> * 나머지 = @ModelAttribute (argument resolver 로 지정해둔 타입 외)
