# redirect

실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다.


* 문법: `redirect:/...` 
* pathVariable 바인딩: `{변수}` => `redirect:/items/{itemId}`
  * 컨트롤러에 매핑된 `@PathVariable`의 값은 redirect에도 사용될 수 있다.
  * `@PathVariable`의 텍스트를 그대로 사용한다.
* RedirectAttributes    
  * redirect할 때 `RedirectAttributes`를 사용하면 return될 화면에 쿼리파라미터로 넘길 수 있다.
  ```
  @PostMapping("/save")
      public String (Long itemId, RedirectAttributes redirectAttributes) {
          redirectAttributes.addAttribute("itemId", itemId);
          redirectAttributes.addAttribute("name", "kim");
          return "redirect:/items/{itemId}";
      }
  ```
  => `url/items/1?name=kim`
  
  
       
 --------------------------------------------------
       
 참고   
 > **redirect vs forward**   
 > `redirect`는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. (요청이 두번, 개발자 도구에서 확인 가능)   
 > `forward`는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다. (요청이 한번).
