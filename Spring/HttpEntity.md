# HttpEntity

#### HttpEntity란 Http 메시지 관련 header와 body의 값들을 하나의 객체로 저장할 수 있도록 역할을 하는 클래스이다.   
* Spring framework에서 지원한다.   
* **Http 메세지의 전반에 대한 데이터와 특히 Header메세지, Body메세지를 편리하게 조회**할 수 있다.   
* HttpEntity를 상속받아 요청과 응답에 특화된 **`RequestEntity`와 `ResponseEntity`** 클래스가 있다.
* ttp 메세지를 읽어오는데 개발자가 따로 캐릭터셋을 지정해주지 않아도 된다. 


### RequestEntity
* HttpMethod, url 정보 등이 추가되어 있으며, 요청에서 사용한다.  
### ResponseEntity
* **응답에 대한 상태코드**를 보낼 수 있다. (상태 코드를 정해놓은 enum클래스를 참고)
* Http 응답 메세지에 바로 데이터를 넣어 보내는것이므로 view를 조회하지 않는다.   

![image](https://user-images.githubusercontent.com/108853290/188786671-98099d86-da11-4e90-8cff-30440ae325d8.png)   

-------------------------------------------------------------  
## 사용 예시

![image](https://user-images.githubusercontent.com/108853290/188784235-6020b34d-41af-459d-9fbd-c61b17a66735.png)    

출력 결과   

```
2022-09-07 12:39:50.940  INFO 16724 --- [nio-8080-exec-4] h.s.b.r.RequestBodyStringController      
: httpEntity=<{"username":"kim", "age":"20"},
              [
                user-agent:"PostmanRuntime/7.29.2",   
                accept:"*/*",   
                postman-token:"d5a6555e-398a-44a5-a989-57dd2ec89e05",
                host:"localhost:8080",
                accept-encoding:"gzip, deflate, br",
                connection:"keep-alive",
                content-length:"30",
                cookie:"JSESSIONID=0811E72560C199F22E8E7C16CCDBA6FF",
                Content-Type:"application/json;charset=UTF-8"
               ]>
```
객체를 통으로 출력해보면 HTTP 요청 메세지와 헤더값에 대한 전반적인 내용을 모두 출력해주는것을 볼 수 있다.   
  
![image](https://user-images.githubusercontent.com/108853290/188784482-d709954b-4e7f-41bc-99b7-bed95501fb0e.png)   
![image](https://user-images.githubusercontent.com/108853290/188784563-7a229c91-ebb4-4d15-8bc5-05d8acd79597.png)   
`.getBody(), .getHeaders()` 메소드를 통해 Http 요청 메세지중에서 따로 뽑아 쓸 수 있다.     


![image](https://user-images.githubusercontent.com/108853290/188785805-df6c7ea6-7eb7-4ac4-9534-fc4c8e2abc89.png)       
![image](https://user-images.githubusercontent.com/108853290/188785979-f7c7d107-ce13-4e25-8062-407e730dcd18.png)   
HttpEntity타입에 객체를 넣으면 알아서 우리가 원하는 형태로 파싱을 해주고 응답도 객체로 나갈 수 있다.     

---------------------------------------------------

> **<<참고>>**  
> HTTP 메시지 바디를 읽어서 문자나 객체로 변환해서 전달해주는데, 이때 **HTTP 메시지 컨버터( HttpMessageConverter )라는 기능을 사용**   
> 
> 응답이 나갈 때 `@ResponeBody`를 사용하면 `@ResponseStatus(HttpStatus.OK)`처럼 애노테이션을 통해 상태코드를 보낼 수 있다. 
> `ResponseEntity` 대신 `@ResponeBody`를 사용하면 Http 메세지에 직접 데이터를 넣어서 보내는것은 동일하지만,    
>  `@ResponseStatus`는 상황에 따라 동적으로 상태값을 변경해 보낼 수 없다. 메소드 레벨에 애노테이션이 붙어 해당 요청에 따른 응답 코드는 동일하게 나간다.   
> 그럴때는 `ResponseEntity`를 사용하자.
