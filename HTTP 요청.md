# HTTP 요청/응답 데이터

> 인프런의 김영한 님의 강의를 바탕으로 공부
  
### HTTP 요청 메세지를 통해 클라이언트에서 서버로 데이터를 전달하는 세가지 방법 ###
* #### GET - 쿼리 파라미터 ####
  * ##### 메세지 바디 없이 url에 쿼리 파라미터를 포함해 전달 #####
  * ex) url?username=hello&age20
  
* #### POST - HTML Form #### 
  * ##### 메세지 바디에 쿼리 파라미터 형식으로 전달 #####
  * content-type: application/x-www-form-urlencoded
  
* #### HTTP message body에 데이터를 직접 담아서 요청 #### 
  * 주로 API에서 사용/ JSON, XML, TEXT
  
> content-type은 HTTP 메세지 바디의 형식을 지정한다.
그래서 메세지 바디가 없는 GET방식은 content-type이 없고
POST 방식은 메세지 바디에 데이터를 담아 보내기때문에 content-type를 지정해줘야한다.


------------------------------

HTTP 메세지의 바디 데이터를 inputStream을 사용해서 직접 읽을 수 있다.
inputStream은 byte 코드를 변환하므로 우리가 읽을 수 있는 String으로 보려면 Charset을 지정해 줘야 함.

```java 
@Override
 protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
   ServletInputStream inputStream = request.getInputStream();
   String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
   
 }
}
```
필요의 경우 이 처럼 코드를 직접 읽을 수 있지만 requeset.parameter() 혹은 JSON데이터의 경우 Jackson, Gson과 같은 JSON변환 라이브러리가 있기때문에 이를 활용하면 된다.


**참고**
> ObjectMapper란?
> Jackson에서 지원하는 클래스; JSON 타입을 사용할 때 해당 응답들을 직렬화/역직렬화 할 때 사용
> * 직렬화 : Obeject -> String
> * 역직렬화 : String -> Object
> 
> JSON 결과를 파싱해서 사용할 수 있는 자바 객체로 변환하려면 Jackson, Gson 같은 JSON변환 라이브러리를 추가해서 사용해야 한다.


-----------------
**참고2**
> application/json은 스펙상 utf-8형식을 사용하도록 정의되어있다. 따라서 application/json;charset-utf-8는 무의미한것.


