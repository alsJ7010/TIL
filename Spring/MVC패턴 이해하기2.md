# MVC패턴 이해하기2


### 첫번째 리팩토링 ###
* 포워드 중복 -> 포워드를 해주는 역할을 공통으로 뽑아내기
* prefix(/WEB-INF/views), suffix(.jsp)의 중복  -> prefix, suffix를 붙여주는 공통 패턴 뽑아내기, 향휴 뷰의 폴더 위치가 이동해도 쉽게 고칠 수 있음
* 사용하지 않아도 무조건 불러와야하는 매개변수 코드(서블릿으로부터 종속성을 가짐)
-> 컨트롤러가 서블릿에 종속적이지 않으면서도 요청, 응답 데이터를 담을 수 있는 Model의 필요성
``` java
HttpServletRequest request, HttpServletResponse response
```
* 항상 상속 받아야하는 HttpServlet -> FrontController에서만 상속받아 서블릿으로부터 종속성을 제거
* 여기저기서 받는 클라이언트 요청 -> 모든 요청은 FrontController에서 먼저 받고 각자의 컨트롤러로 매핑되도록 설계


=> 매번 컨드롤러에서 request, response를 받기엔 서블릿에 종속적이므로 모든 요청은 먼저 프론트 컨트롤러에서 받아 모델 객체에 넣어주고
view Path의 역할을 해주고 렌더링 해주는 역할이 필요
-----------------

프론트 컨트롤러의 작성과 핵심
```java
@Override
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    ...

    Map<String, String> paramMap = createParamMap(request);   
    ModelView mv = controller.process(paramMap);    
    /* 들어온 request값들을 맵에 먼저 담고 해당 컨트롤러에 전달
     * 전달된 값을 가지고 프로세스를 수행한 후
     * 뷰 이름과 모델을 가지고 되돌아온다.
     */
     
    String viewName = mv.getViewName();
    MyView view = viewResolver(viewName);

    view.render(mv.getModel(), request, response);
    // 공통된 뷰 이름을 모두 붙여주고 필요한 값과 함께 해당 뷰로 렌더링 
}
```

* 서블릿과 비슷한 모양을 가지면서 Model을 담을 수 있고 view이름을 같이 전달할 수 있는 인터페이스를 도입
리턴값을 모델과 뷰 이름을 담을 수 있게 강제한다.
```java
public interface ControllerV3 {
    ModelView process(Map<String, String> paramMap);
}
```

```java
@Override
public ModelView process(Map<String, String> paramMap) {
    List<Member> members = memberRepository.findAll();
    ModelView mv = new ModelView("members");
    mv.getModel().put("members", members);
    return mv;
}
```

```java
public void render(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    model.forEach((key, value) -> request.setAttribute(key, value));
    RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
    dispatcher.forward(request, response);
}
```
-> model에 담긴 데이터를 뷰에 전달할 수 있게 담아주고 view경로로 포워딩 해주는 공통 역할을 뽑아낸다.

* viewResolver의 역할 : 공통된 패턴을 잡아준다.
```java
private MyView viewResolver(String viewName) {
    return new MyView("/WEB-INF/views/" + viewName + ".jsp");
}
```


---------------------
