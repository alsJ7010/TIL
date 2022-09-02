# DispatchServlet을 통해 보는 스프링 구조


 ![image](https://user-images.githubusercontent.com/108853290/187870504-ff3a08b8-88d0-474b-9a15-f3143d9ea382.png)   
스프링 구조를 그림으로 간단히 보여주는 이미지이다.
모든 요청은 일단 FrontController인 `DispatcherServlet`에서 시작된다. 
요청을 처리하기 위해서는 `DispatcherServlet`이 미리 담아두어야 할 값들에 대해서, 실행 로직에 대해서 스프링의 코드로 좀 더 자세히 봤다.

### 요청의 시작
먼저 서블릿이 호출되면 `HttpServlet`이 제공하는 `service()`가 호출된다.   
스프링MVC에서 `HttpServlet`는 `DispatchServlet`이 상속받고 있는 `FrameworkServlet`에서 `service()`를 오버라이드 해두었고   
`FrameworkServlet.service()`를 시작으로 `DispatchServlet`의 **`doDispatch()`** 메소드를 실행하며 여기에 핵심 로직이 다 들어있다.


![image](https://user-images.githubusercontent.com/108853290/187873461-d1694def-c17e-42c5-bb53-a9f1582854ab.png)      
**`DispatcherServlet` 초기화 단계에서 스프링 컨테이너를 매개변수로 받아서 `initStrategies` 메소드를 실행**하게 된다.   
여기서 요청에 필요한 여러 값들을 초기화해둔다.   
  
  
### * HandlerMappings

![image](https://user-images.githubusercontent.com/108853290/187939752-add866ff-3093-4789-bb05-0556e3b967e8.png)   
![image](https://user-images.githubusercontent.com/108853290/187874993-e9d4710d-a256-4228-9373-0d0900947bbe.png)   
그 중에서 `initHandlerMappings`이라는 메소드를 실행한다. 이 메소드는 **들어온 요청을 처리할 수 있는 핸들러들을 List에 담아두는 역할**을 한다.   
`HandlerMapping` 인터페이스를 구현한 모든 class를 빈으로 등록하고 `matchingBeans`에 넣은 뒤 value값들을 뽑아 HandlerMappings(List)에 넣는다.   
   
`matchingBeans` Map에서 value값의 타입인 `HandlerMapping`는 인터페이스이다.   
![image](https://user-images.githubusercontent.com/108853290/187875546-6aaf2245-30fb-489c-b477-b4969e4e9a2a.png)   
`HandlerMapping`인터페이스는 요청과 핸들러 개체 간의 매핑을 정의하는 인터페이스이다.  


### * HandlerAdapters
**여러타입의 요청을 받고 응답을 주기 위해서는 DispatcherServlet과 핸들러(컨트롤러) 사이에 타입을 맞춰줄 단계를 하나 두어야한다**. 마치 110v를 쓰기위해 중간에 어탭터를 사용해 맞추는것처럼!
  
![image](https://user-images.githubusercontent.com/108853290/187947953-e1adbb56-40cf-479a-8a3c-84ca1676a499.png)   
![image](https://user-images.githubusercontent.com/108853290/187948181-ba19b8ad-2a07-4ff2-a44e-94dfdd6b243e.png)   
**마찬가지로 `initHandlerAdapters` 메소드를 통해 핸들러 어댑터를 초기화**한다.    
`handlerAdapters`에는 이런 `HandlerAdapter`들이 담겨있다.
  
**`HandlerAdapter`타입도 인터페이스로 구현**되어 있다.   
![image](https://user-images.githubusercontent.com/108853290/187957645-23df374a-6ed9-4512-bca5-30f752232f99.png)   
이 인터페이스에는   
1) **매개변수로 들어온 핸들러의 인스턴스를 확인해 해당 HandlerAdapter가 지원할 수 있는지 여부를 boolean 타입으로 반환하는 `supports`메소드**와   
2) **request, response객채와 핸들러(컨드롤러)를 매개변수로 받아 핸들러의 로직을 실행시킬 수 있는 `handle` 메소드**를 담고 있다.    
  * 컨트롤러를 실행하면 `ModelAndView` 타입으로 동일하게 반환하여 `DispatcherServlet`에서 사용할 수 있게 한다.
  * 리턴 타입(`ModelAndView`)을 맞춘덕분에 어떤 타입의 요청과 응답에도 `DispatcherServlet`에서 동일한 로직을 실행 할 수 있게된다. = **(어떤 타입이던지 처리할 수 있다!!)**

   
이렇게 필요한 핸들러들과 핸들러 어댑터들을 담아놓으면 이후의 요청에 따라 핸들러를 가져오고, 그 핸들러를 실행할 수 있는 핸들러 어댑터를 찾아오게된다.   
![image](https://user-images.githubusercontent.com/108853290/187956768-e2f18121-28cc-401b-8c35-035367cd22b8.png)       
  
### * 요청에 맞는 핸들러 찾기_getHandler
![image](https://user-images.githubusercontent.com/108853290/187956181-eb791b11-a0e3-4675-a7d6-dfe48c1d322f.png)   
**request객체를 가지고 HandlerMappings에 담겨져있는만큼 for문을 돌며 해당 요청에 맞는 handler를 찾아 반환**한다.  

### * 핸들러를 실행할 수 있는 어댑터 찾기_getHandlerAdapter
![image](https://user-images.githubusercontent.com/108853290/187956926-3fb0223c-5ab4-4f1a-9e0f-a677682acfab.png)   
그러면 **그 handler를 가지고 handler를 실행시킬 수 있는 `HandlerAdapter`를 찾는다**.   
`HandlerAdapters`에서 for문을 돌며   
supports를 통해 어댑터 지원 여부를 판단하고 true면 handle 메소드를 실행하며 컨트롤러 로직을 실행한다.   

handler를 처리할 수 있는 HandlerAdapter가 없다면 예외를 던진다   
  
  
### * 핸들러 실행과 동일한 타입 반환
![image](https://user-images.githubusercontent.com/108853290/187960391-fd888b68-083a-45d7-819d-4751a78cbd6c.png)   

위의 로직에서 **요청에 맞는 handlerAdapter를 찾으면 매개변수로 받은 핸들러와 함께 handle메소드를 실행해 컨트롤러 로직을 실행**한다.   
그리고 **ModelAndView 를 리턴값으로 반환** 한다.
  
 ### 이후 로직
ModelAndView를 가지고 viewResolver를 거치면 View를 반환하게 되는데    
View 인터페이스에는   
컨트롤러 로직 실행을 마치고 결과값이 담겨있는 model매개변수와 request, response객체를 가지고 최종적으로 요청에 대한 응답이 나가게 하는 `render`메소드가 있다.   
![image](https://user-images.githubusercontent.com/108853290/187967849-0e6ba83c-9b2b-47ee-9534-f5589ab4aa55.png)   
`processDispatchResult` 속 render 메소드를 통해 최종적으로 요청과 응답에 대한 싸이클이 마무리된다.


