# HTTP 메세지 컨버터
> 인프런 김영한님의 강의 [스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술] 참고



![image](https://user-images.githubusercontent.com/108853290/180419621-544a673b-ff63-42a7-901b-73183be2b716.png)

* **HTTP의 BODY에 문자 내용을 직접 반환**
* **`viewReslver` 대신에 `HttpMessageConverter` 가 동작**
* 여러 타입에 대한 각각의 메세지 컨버터가 기본으로 등록되어 있음

-------------------------------------------    

스프링 부트 기본 메세지 컨버터 (일부)
```
0 = ByteArrayHttpMessageConverter  (byte[] 데이터를 처리)
1 = StringHttpMessageConverter     (String 문자로 데이터를 처리)
2 = MappingJackson2HttpMessageConverter
```
* 대상 **클래스 타입**과 *미디어 타입(content-type)** 두가지를 체크해서 결정.
* 우선순위대로 돌아가면서 걸정한다.    


||클래스 타입|미디어 타입|
|:---|---:|:---:|
|ByteArrayHttpMessageConverter|byte[]|*/*|
|StringHttpMessageConverter|String|*/*|
|MappingJackson2HttpMessageConverter|객체 또는 HashMap|application/json|


**참고**
> HTTP 메시지 컨버터는 HTTP 요청, HTTP 응답 둘 다 사용된다.
> 모든 요청과 응답에서 HTTP 메세지 바디에 데이터가 담겨있으면 content-type이 지정이 되어있어야 한다.





---------------------------------------------------------------     
  
    
    

**그렇다면 스프링에서 이 메세지 컨버터가 쓰이는 곳은??**


### 컨트롤러에서 수많은 형태의 파라미터들을 받을 수 있었던 이유 ### 
  `
  public String requestBodyJsonV2(@RequestBody String messageBody) throws IOException { ... }  
  `
  * ex) @RequestBody, @HttpEntity, Model, 객체 등    
  * **ArgumentResolver때문!**
  * 인터페이스로 제공됨, **각각의 파리미터에 타입에 대한 맞춤 리졸버가 존재**
  * **컨트롤러를 호출하는 핸들러에서 해당 파라미터가 필요하다고 하면 먼저ArgumentResolver에서 해당 파라미터를 지원하는지 확인 한 후 만들어서 컨트롤러에 파라미터를 주는것!**
  * **그런데 ArgumentResolver를 동작하며 파라미터를 만들어주는 중에 HTTP message body에 있는걸 바로 처리해야된다고 하면 메세지 컨버터가 동작해서 만들어준다. 이 때 메세지 컨버터가 동작한다.**
  
  ![image](https://user-images.githubusercontent.com/108853290/180602514-a49e0b15-4c3a-4b59-b67f-baae64c39aa4.png)

  
**주의**  
> ArgumentResolver는 Argument를 찾는거고    
> ArgumentResolver중에서 HTTP message body에 있는걸 바로 처리해야된다고 하면 메세지 컨버터가 동작         

      

### 컨트롤러에서 수많은 형태의 리턴값을 보낼 수 있었던 이유 ###
* **ReturnValueHandler 때문**
* 응답도 위에 ArgumentResolver와 비슷한 원리
* 리턴 타입에 대한 여러가지 ReturnValueHandler가 있고 리턴 타입에 따라 맞는 응답 결과를 만들고
* 이 때 필요시에 HTTP 메세지 컨버터를 호출해 응답 결과를 만드는것이다.
