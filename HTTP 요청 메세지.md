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
