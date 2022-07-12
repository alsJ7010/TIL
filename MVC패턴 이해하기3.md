# MVC패턴 이해하기

### 두번째 리팩토링 ###

**목적**

* 컨트롤러에서 항상 ModelView객체를 생성하고 반환하지 않고 viewName만 반환하도록 한다.

**컨트롤러의 더욱 간결한 코드**


**방법**

* **컨트롤러 인터페이스에 model객체 파라미터를 추가, 리턴값은 뷰 이름**

```java
public interface ControllerV4 {
   String process(Map<String, String> paramMap, Map<String, Object> model);
}
```

구현 컨트롤러
```java
@Override
 public String process(Map<String, String> paramMap, Map<String, Object> model) {
   String username = paramMap.get("username");
   int age = Integer.parseInt(paramMap.get("age"));
   Member member = new Member(username, age);
   memberRepository.save(member);
   model.put("member", member);
   return "save-result";
 }
```

=> model.put("member", member)
모델이 파라미터로 전달되기 때문에, 모델을 직접 생성하지 않아도 되고 코드가 간결해진다.

프론트 컨트롤러

```java
@Override
 protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  
  ...

   Map<String, String> paramMap = createParamMap(request);
   Map<String, Object> model = new HashMap<>(); //추가
   String viewName = controller.process(paramMap, model);
   MyView view = viewResolver(viewName);
   view.render(model, request, response);
 }
```

=> Map<String, Object> model = new HashMap<>()

모델 객체를 프론트 컨트롤러에서 생성해서 넘겨주는것으로 수정.
