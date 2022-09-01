# FrontController의 이해

MVC 패턴을 적용한 덕분에 컨트롤러의 역할과 뷰를 렌더링 하는 역할을 명확하게 구분할 수 있다.
하지만 그 다음에!

```java
@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {
   
   @Override
   protected void service(HttpServletRequest request, HttpServletResponse response)
   throws ServletException, IOException {
     
     ...
     
     String viewPath = "/WEB-INF/views/members.jsp";
     RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
     dispatcher.forward(request, response);
 }
}
```
### 해당 코드의 단점 ###
* 포워드 중복
* prefix(/WEB-INF/views), suffix(.jsp)의 중복
```java
String viewPath = "/WEB-INF/views/members.jsp";
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```
* 사용하지 않아도 무조건 불러와야하는 매개변수 코드
```java
HttpServletRequest request, HttpServletResponse response
```
* 항상 상속 받아야하는 HttpServlet
* 여기저기서 받는 클라이언트 요청

### 문제 ###
결과적으로 공통처리가 어렵다는 문제가 발생한다.

### 해결 ###
**FrontController의 도입** (스프링에서는 DispatcherServlet의 역할!)


> FrontController 패턴의 특징
> * FrontController 서블릿 하나로 클라이언트의 요청을 받음. 모든 요청은 일단 이 FrontController에서 먼저 받는다.
> * 그 이후 FrontController가 요청에 맞는 컨트롤러를 찾아서 호출해준다.
> * 입구를 하나로; 공통 처리 가능
> * 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨

**프론트 컨트롤러 도입 전**

![image](https://user-images.githubusercontent.com/108853290/178140955-62b395ad-207c-4046-854d-65789bab7d16.png)

**프론트 컨트롤러 도입 후**

![image](https://user-images.githubusercontent.com/108853290/178140659-656f8bb6-6e87-4072-a652-d74403dd0baa.png)

-------------------------
참고
> **redirect vs forward**
> redirect는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect경로로 다시 요청.
> 호출이 총 두번된다. 따라서 클라이언트가 인지할 수 있고 url 경로도 실제로 변경된다.
> forward는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.
