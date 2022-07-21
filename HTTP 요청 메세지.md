# HTTP 요청 메세지 - 단순 텍스트 받아오기

> 클라이언트에서 서버로 데이터를 보낼 때는 세가지 방법밖에 없다.   
> GET 방식 전송, POST form 전송, Http messageBody에 담아서 전송.   
> 요청 파라미터가 온다면 `@RequestParam`과 `@ModelAttribute`로 받을 수 있지만 Http messageBody에 담겨오는 데이터는 해당 애노테이션을 쓸 수 없다.   
> **Http messageBody에 담겨오는 데이터를 읽기 위해서는**   

`public String requestBodyStringV4(**@RequestBody** String messageBody) { ... }`

### @RequestBody ###
`@RequestBody`를 사용하면 HTTP messageBody 정보를 편리하게 조회 할 수 있다.

### @RequestBody ###
`@ResponseBody` 를 사용하면 응답 결과를 HTTP 메시지 바디에 직접 담아서 전달할 수 있다.

`
> **요청 파라미터 vs HTTP 메시지 바디**  
> 요청 파라미터를 조회하는 기능: @RequestParam , @ModelAttribute 
> HTTP 메시지 바디를 직접 조회하는 기능: @RequestBody 
`



**참고**
> * InputStream(Reader): HTTP 요청 메시지 바디의 내용을 직접 조회
> * OutputStream(Writer): HTTP 응답 메시지의 바디에 직접 결과 출력
> * HttpEntity: HTTP header, body 정보를 편리하게 조회, 응답에도 사용 가능
등의 방식으로도 단순 텍스트를 받을 수 있지만 최종적으로 편리하게 쓸 수 있는 기능은 @RequestBody, @ResponseBody!


---------------------------------------------------------------------

# HTTP 요청 메세지 - JSON

> JSON 타입으로 올 경우

1) 서블릿 `request.getInputStream()`으로 직접 받아와 `ObjectMapper`로 HelloData객체에 직접 파싱해주는 방법 

  `request.getInputStream()`으로 받아와 `StreamUtils.copyToString`를 사용해 바이트형태의 메세지를 utf-8 스트링으로 받아온다. 후에 objectMapper.readValue를 사용해 HelloData 객체에 직접 파싱해준다. 
  
  
2) `@RequestBody ` : 직접 HTTP 메세지에서 데이터를 꺼내 자바 객체로 변환하는 방법


```
public String requestBodyJsonV2(@RequestBody String messageBody) throws IOException {   
  HelloData data = objectMapper.readValue(messageBody, HelloData.class);    
  ...   
}
```   
받아온 messageBody를 바로 objectMapper.readValue로 파싱해서 사용

3)  **문자로 받고 객체 파싱하는것까지 한번에 해주는 방법!**   
   `
   public String requestBodyJsonV3(@RequestBody HelloData data) { ... }
   `
   * JSON 데이터가 그대로 HelloData객체에 들어옴.
   * **@RequestBody는 생략 불가능** => 생락하면 ` @ModelAttribute` 가 생략된것으로 간주해 기본값이 들어오게 됨!
   * **`HttpEntity` , `@RequestBody` 를 사용하면 HTTP 메시지 컨버터가 HTTP 메시지 바디의 내용을 우리가 원하는 문자나 객체 등으로 변환해준다.** HTTP 메시지 컨버터는 문자 뿐만 아니라 JSON도 객체로 변환해준다.


참고
* HttpEntity로 가져와 .getBody로도 사용할 수 있음
* 응답도 JSON형태로 응답 가능

> **@RequestBody 요청**   
> JSON 요청 -> HTTP 메시지 컨버터 -> 객체    

> **@ResponseBody 응답**    
> 객체 -> HTTP 메시지 컨버터 -> JSON 응답   
