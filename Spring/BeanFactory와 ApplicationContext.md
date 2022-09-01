# BeanFactory와 ApplicationContext
 
두 가지 모두 스프링 빈을 생성하고 관리하는 **스프링 컨테이너**이다.   
DI의 관점에서 초점을 두어 설명할 때 `BeanFactory`를 쓰기도 한다. 

**`BeanFactory`는 스프링 컨테이너의 최상위 인터페이스**이며 **스프링 빈을 관리하고 조회**하는 역할을 담당한다.  
  
![image](https://user-images.githubusercontent.com/108853290/183077108-c8035b3e-c06f-4474-a2a1-c7d68e8a9acc.png)   
![image](https://user-images.githubusercontent.com/108853290/183077198-41bbb96c-7054-4263-be2b-594b3ffbde3d.png)   

   
위의 사진처럼 `ApplicationContext`는 `BeanFactory`의 모든 기능을 상속받아서 제공받는다.
그러나 빈을 관리하고 검색하는 기능을 이미 BeanFactory에서 모두 제공하는것이다.
그렇다면 왜 굳이 `ApplicationContext`를 써야할까?
  

개발에 필요한 그 외의 기능을 `ApplicationContext`가 지원해주기 때문이다.   

* 메시지소스를 활용한 국제화 기능
  * 다국어 지원에 따른 메세지 기능
* 환경변수
  * 로컬, 개발, 운영등을 구분해서 처리, 소스 설정 및 프로퍼티 값을 가져오게 해줌
* 편리한 리소스 조회
  * 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회
 
  
-------------------------------------------------------------

또한 **빈의 생성 시점에서의 차이**도 있다.
  
 
### 빈 생성 시점
## 1) BeanFactory - Lazy loading
`BeanFactory` 는 `.getBean()`을 사용하여 **빈을 호출 할 때 빈을 생성**한다.  
`BeanFactory factory = new ClassPathXmlApplicationContext("bean-factory-test.xml");` -> 이 시점에는 빈들이 생성되지 않는다.   
`DiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);` -> 빈을 조회하는 시점에서 빈이 생성된다.

## 2) ApplicationContext - pre loading
`ApplicationContext`는 **컨테이너 실행시점에(런타임) 모든 빈을 생성**해 놓는다.   
`AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);` -> 이 시점에서 모든 빈들을 미리 생성!
