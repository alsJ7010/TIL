# HandlerMapping과 HandlerAdapter
> 해당 정리는 김영한님의 인프런_[스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술]을 참고하였습니다.

> **HandlerMapping과 HandlerAdapter는 모두 인터페이스**이다.   
> 요청에 맞는 컨트롤러를 실행 시키기 위해서 HandlerMapping과 HandlerAdapter를 인터페이스로 두고 여러 타입으로 구현체를 구현하도록 했다.   
> 스프링에서는 이미 필요한 대부분의 HandlerMapping과 HandlerAdapter를 모두 구현해 놨다.    
> 각각의 구현체들은 우선순위가 있으며 하나씩 비교해가며 요청에 적절한 구현체를 찾게 된다.   

### HandlerMapping 

|우선순위|이름|설명|
|------|---|---|
|0|RequestMappingHandlerMapping|애노테이션 기반의 컨트롤러인 @RequestMapping에서 사용|
|1|BeanNameUrlHandlerMapping|스프링 빈의 이름으로 핸들러를 찾는다.|
...

### HandlerAdapter
|우선순위|이름|설명|
|------|---|---|
|0|RequestMappingHandlerAdapter| 애노테이션 기반의 컨트롤러인 @RequestMapping에서 사용|
|1|HttpRequestHandlerAdapter|HttpRequestHandler 처리|
|2|SimpleControllerHandlerAdapter|Controller 인터페이스(애노테이션X, 과거에 사용) 처리|
...  

### 예시1
![image](https://user-images.githubusercontent.com/108853290/188144256-cec3bdbb-bd85-485c-88ae-ddd4c5b9d002.png)   
`url/request-mapping`의 주소로 접근을 하게 된다면 먼저  
1) `HandlerMappings`리스트를 돌며 핸들러를 조회한다.   
2) `@RequestMapping` 애노테이션을 사용한 컨트롤러 기반이므로 `RequestMappingHandlerMapping`가 실행에 성공하고 `RequestMappingController`를 반환한다.(핸들러 찾음)   

이제 핸들러를 찾았으니 이 핸들러를 가지고 짝궁 핸들러 어댑터를 찾는다.   

3)  `HandlerAdapter`의 `supports()` 를 순서대로 호출한다. (해당 핸들러의 인스턴스가 어댑터와 맞는지 체크)  
4)  애노테이션 기반의 컨트롤러인 @RequestMapping을 사용했으므로 `RequestMappingHandlerAdapter`가 핸들러 어댑터가 된다.

6) 찾은 핸들러(`RequestMappingController`)를 매개변수로 `RequestMappingHandlerAdapter`에 넘겨 해당 핸들러 로직을 실행한다. 


### 예시2
![image](https://user-images.githubusercontent.com/108853290/188150001-fe2682dc-3612-430f-8546-2dd6398f2c34.png)   
![image](https://user-images.githubusercontent.com/108853290/188150863-06a25cdb-3ac1-4dcb-88cd-8a5314387463.png)

**빈 등록 이름을 따로 설정** 해놓았고 **Controller인터페이스를 상속받아 구현한 컨트롤러**이다.
Controller인터페이스를 상속받아 구현한 컨트롤러는 지금은 쓰지 않는 방식이다.

핸들러 찾기   
1) `HandlerMappings`리스트를 돌며 핸들러를 조회한다.     
2) 빈 이름으로 핸들러를 찾아야 하므로 `BeanNameUrlHandlerMapping`가 실행에 성공하고 핸들러인 `OldController` 를 반환한다.   

핸들러 어댑터 찾기  

3) `HandlerAdapter`의 `supports()` 를 순서대로 호출한다.       
4) Controller 인터페이스를 지원하는 핸들러 어댑터는 `SimpleControllerHandlerAdapter`이므로 해당 어댑터가 대상이 된다.   
5) 찾은 핸들러(`OldController`)를 매개변수로 `SimpleControllerHandlerAdapter`에 넘겨 해당 핸들러 로직을 실행한다. 
이 때는 Controller인터페이스를 상속받아 재정의한 `handleRequest()`메소드가 실행 메소드가 된다.      


------------------------------------------

이런 과정을 거치며 Handler 와 HandlerAdapter이 선택이 된다.   
대부분 `@RequestMapping`의 방식을 써서 컨트롤러를 사용하며    
이를 처리 할 수 있는 `RequestMappingHandlerMapping` , `RequestMappingHandlerAdapter`가 우선순위가 가장 높다.
