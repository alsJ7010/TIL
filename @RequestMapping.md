# @RequestMapping


* `RequestMapping`은 URL 매칭뿐만 아니라 HTTP Method도 구분할 수 있다. => `@GetMapping, @PostMapping`
* ex) method=RequestMethod.GET

**이런 기능을 아예 애노테이션으로 편리하게 사용 가능**

 `@GetMapping`
 안으로 들어가보면 내장되어있는 코드 
 `@RequestMappinng(method = RequestMethod.GET)`   
 
 
 **참고**
> 무작정 @RequestMapping으로 매핑하는것 보다는 **HTTP 메소드를 살린 애노테이션으로 매핑하는것이 더 좋은 코드**!!

-----------------------------------------------------------------------------------------------------------------

* 단순 `@RequestMapping` 요청   
  * 단순히 `@RequestMapping` 만으로만 요청을 매핑하면 Http 메서드와 무관하게 호출. 모든 Http요청이 허용된다.


* 특정 Http method만 허용   
  * `@RequestMapping(value = "/url", method = RequestMethod.GET)`
  * 다른 메서드로 요청 시 405에러 반환  
  * GET, HEAD, POST, PUT, PATCH, DELETE
  
  
* 특정 Http 메서드 매핑 축약   
  * `@GetMapping(value = "/url")`
  * 좀 더 직관적인 매핑
  
  
* PathVariable(경로 변수) 사용  
  ```java
  @GetMapping("/url/{userId}")
  public String mappingPath(@PathVariable("userId") String data) {
      log.info("mappingPath userId={}", data);
      return "ok";
  }
  ```
  
  * 매핑 url을 변수로 사용 가능
  * 변수명이 같으면 생략 가능 `@PathVariable String data`
  * 다중 사용 가능  `(@PathVariable String name, @PathVariable Long id)`
  
  
* 특정 파라미터 조건 매핑
  * `@GetMapping(value = "/url", params = "user=a")`
  * url에 해당 파라미터가 있어야 호출이 됨
  
  
* 특정 헤더 조건 매핑
  * `@GetMapping(value = "/url", headers = "mode=debug")`
  * 위의 매핑과 비슷하지만 Http 헤더를 사용.

   
   ---------------------------------------------------
   

