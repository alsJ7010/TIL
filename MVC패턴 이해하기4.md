# MVC패턴 이해하기 마지막!

### 마지막 리팩토링 ###
;유연한 컨트롤러 

### Adapter 개념의 도입 ###

![image](https://user-images.githubusercontent.com/108853290/178702259-8707fbbb-1a8a-4807-b323-3d6661c04593.png)

의 구조를 보면 먼저 모든 요청을 FrontController에서 받고 해당하는 컨트롤러에서 프로세스를 수행 한뒤 다시 FrontController를 거친다.
하지만 해당 컨트롤러의 리턴값이 다르면? **여러 타입의 컨트롤러를 받을순 없을까**?
그 해답이  **Adapter의 도입으로 해결**이 된다.

----------------------------

**어댑터 패턴**

서로 다른 리턴값을 가지는 Contoller의 인터페이스를 위해 Adapter가 필요하다.
어댑터 패턴을 사용해서 FrontController가 다양한 방식의 컨트롤러를 처리할 수 있도록!


![image](https://user-images.githubusercontent.com/108853290/178704032-2b791321-def8-4e37-8ce0-dd847ce2b2fa.png)
* 핸들러 어댑터:
  * 요청된 어댑터가 해당 컨트롤러를 처리할 수 있는지 판단하는 역할을 하고
  * 처리할 수 있으면 해당하는 컨트롤러를 호출해서 프로세스를 실행하도록 한다. 
  

```
@Override
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    Object handler = getHandler(request);   //해당 요청에 따라 알맞은 controller를 반환하면
    if (handler == null) {
        response.setStatus(HttpServletResponse.SC_NOT_FOUND);
        return;
    }

    MyHandlerAdapter adapter = getHandlerAdapter(handler);    //핸들러 어댑터가 해당 컨트롤러를 처리할 수 있는지 한번 더 확인 ,

    ModelView mv = adapter.handle(request, response, handler);    //해당 어댑터에 맞는 컨트롤러(handler)를 넘기면 프로세스 실행 후 리턴값만 맞춰주면 됨. 
                                                                  //***프론트 컨트롤러가 실제 컨트롤러를 호출하는게 아니라 어댑터를 통해 실제 컨트롤러를 호출된다.***

    String viewName = mv.getViewName();
    MyView view = viewResolver(viewName);

    view.render(mv.getModel(), request, response);
}
```
```java
private Object getHandler(HttpServletRequest request) {
  String requestURI = request.getRequestURI();
  return handlerMappingMap.get(requestURI);   //return값은 Controller가 들어가게됨
}
```

```
public interface MyHandlerAdapter {
    boolean supports(Object Handler);
    ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws ServletException, IOException;
}
```
### 다형성의 중요성!!! ###
어떤 컨트롤러가 들어오는지는 FrontController입장에서는 모른다. 단지 인터페이스를 사용하여 핸들러 어댑터를 만들어주고 리턴값만 잘 맞춰주게 설계하하면
FrontController입장에서는 어떤 구현체가 들어올지는 모르지만 똑같은 타입에 따라 똑같이 실행하기만 하면된다.
MyHandlerAdapter 인터페이스를 사용해 컨트롤러 클래스를 새로 만들어주면 확장 범위가 매우 넓어진다.


**컨트롤러(Controller) 핸들러(Handler)**
이전에는 컨트롤러를 직접 매핑해서 사용했다. 그런데 이제는 어댑터를 사용하기 때문에, 컨트롤러 뿐만
아니라 어댑터가 지원하기만 하면, 어떤 것이라도 URL에 매핑해서 사용할 수 있다.
