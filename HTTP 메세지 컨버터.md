# HTTP 메세지 컨버터
> 인프런 김영한님의 강의 [스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술] 참고``



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
