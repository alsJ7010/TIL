# TIL

* **@RequestParam 사용**

  * 스프링은 HTTP요청 파라미터를 @RequestParam으로 받을 수 있다.
  * `@RequestParam("username")` 과 `request.getParameter("username")` 는 같은 코드
  * GET 방식과 POST 방식을 모두 지원한다.
  
* **@RequestMapping => @GetMapping, @PostMapping**
* `RequestMapping`은 URL 매칭뿐만 아니라 HTTP Method도 구분할 수 있다. 
* ex) method=RequestMethod.GET

    => **이런 기능을 아예 애노테이션으로 편리하게 사용 가능**
    
    
`@GetMapping` 
안으로 들어가보면 내장되어있는 코드 `@RequestMapping(method = RequestMethod.GET)`

무작정 @RequestMapping으로 매핑하는것 보다는 **HTTP 메소드를 살린 애노테이션으로 매핑하는것이 더 좋은 코드**!!
