# Spring의 동작 방식과 Controller 인터페이스

**Spring의 구조**

![image](https://user-images.githubusercontent.com/108853290/178952255-73226783-2811-4620-a735-5543e830de4f.png)

**동작 순서**
1) **핸들러 조회** : 핸들러 매핑을 통해 들어온 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
  (스프링 부트는  DispacherServlet 을 서블릿으로 자동으로 등록하면서 모든 경로( urlPatterns="/" )에
대해서 매핑한다.)
2) **핸들러 어댑터 조회** : 조회된 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
3) **핸들러 어댑터 실행** : 핸들러 어댑터를 실행한다.
4) **핸들러 실행** : 핸들러 어댑터가 실제 핸들러(컨트롤러)를 실행한다.
5) **ModelAndView 반환** : 핸들러는 ModelAndView로 변환해서 반환한다.
6) **viewResolver 호출** : 뷰 리졸버를 찾고 실행한다.
7) **View 반환** : 뷰 이름 패턴을 맞추고 렌더링한다.


**DispatcherServlet**

* DispatcherServlet에서 모든 요청을 제일 먼저 받는다. 부모클래스를 타고타고 들어가보면 HttpServlet를 상속받기 때문이다.


-----------------------------
스프링에서도 핸들러 매핑과 핸들러 어댑터가 있다. 
* **HandlerMapping**
  * 핸들러 매핑에서 요청된 컨트롤러를 찾을 수 있어야 한다. (어떤 요청에서 어떤 컨트롤러를 사용하는지!)
  
* **HandlerAdapter**
  * 핸들러 매핑을 통해서 찾은 핸들러를 실행할 수 있는 핸들러 어댑터가 필요하다.

스프링에서 가장 우선순위가 높은 핸들러 매핑과 핸들러 어댑터는 
`RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`이다.

> RequestMappingHandlerMapping은 **스프링 빈 중에서** `@RequestMapping` 또는 `@Controller`가 클래스 레벨에 붙어있는 경우에 매핑 정보로 인식

------------------------------------------------
`@Controller`
* 스프링이 자동으로 스프링 빈으로 등록한다. 

  => 내부에 @Copnent 애노테이션이 있어서 컴포넌트 스캔의 대상이 되기 때문(service나 repository도 같은 이유).
* 스프링 MVC 에서 애노테이션 기반 컨트롤러로 인식

  => **해당 컨트롤러는 RequestMappingHandlerMapping에서 핸들러 정보임을 인식하고 꺼낼 수 있는 대상이라는 것**
  (스프링입장에서 스프링이 인식할 수 있는 핸들러임을 찾을 수 있는 방법)
  
`@RequestMapping` 
* 요청 정보를 매핑한다.
* 스프링은 이 어노테이션을 통해 유연한 컨트롤러를 지원한다.
