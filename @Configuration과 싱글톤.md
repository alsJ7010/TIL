# Configuration과 싱글톤
> 해당 내용은 김영한님의 인프런_[스프링 핵심 기술]을 바탕으로 합니다.  


**스프링 컨테이너는 빈의 싱글톤을 보장**해준다고 했다.  
이전에 [스프링 컨테이너와 스프링 빈](https://github.com/MJeong00/TIL/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%99%80%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EB%B9%88.md)
에서 `@Configuration`과 `@Bean` 애노테이션을 사용해 스프링 컨테이너에 빈을 등록한다고 했는데

![image](https://user-images.githubusercontent.com/108853290/183563832-ce74dc86-9ced-45c2-9a3a-85565418d09a.png)  
위의 코드를 보면 `memberRepository()`가 **총 세번 호출**이 된다.  
즉, 위의 코드로 봤을 땐 `new MemoryMemberRepository()`가 세 번 호출 된것처럼 보인이고 객체도 3개가 생성된것처럼 보인다.  

![image](https://user-images.githubusercontent.com/108853290/183565246-5a13c2cc-2997-44a4-9111-ed393844a52f.png)  
하지만 `memberRepository`가 쓰이는 클래스에서 `memberRepository`의 참조값을 가져와 출력해보면 **같은 객체가 사용됨**을 볼 수 있다.

그러면 스프링 컨테이너에서 memberRepository객체는 어떻게 싱글톤이 보장되는걸까?  

  
  
바로 **`@Configuration` 때문**이다.   
`@Configuration`이 적용되면 스프링은 AppConfig를 상속받은 새로운 인스턴스를 만들어낸다.   
![image](https://user-images.githubusercontent.com/108853290/183568181-c30e7cb1-2e0a-4b64-93db-466f5f59f319.png)  
스프링 컨테이너에서 빈으로 등록된 AppConfig의 클래스 타입을 출력해보면  

![image](https://user-images.githubusercontent.com/108853290/183568315-6667e71d-f57d-43d6-b857-f76dce716537.png)
  
`hello.core.AppConfig` 가 아닌 다른것이 출력된다.  
이는
### `@Configuration`이 붙은 파일을 스프링에서 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다!   
![image](https://user-images.githubusercontent.com/108853290/183568647-f34e208d-040b-4313-a1ed-7aa51e2c053b.png)   

**CGLIB라이브러리가 사용되면     
@Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면
생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어지고 그 덕분에 싱글톤이 보장되는 것**이다.

---------------------------------------  

그렇다면 만약 `@Configuration` 을 적용하지 않고, `@Bean` 만 적용하면 어떻게 될까?      
`@Configuration`이 없는 AppConfig는 그저 순수한 자바 코드를 실행하게 된다. 
`memberRepository()`가 호출되면 `new MemoryMemberRepository()`를 호출할 뿐이다. 이것은 스프링이 관리해준 빈을 가져온것이 아니라 새로 객체가 만들어진것이다.   
그래서 memberRepository가 사용되는 클래스에서 참조값을 가져와 출력해보면 각각 다른 인스턴스임을 볼 수 있다.
![image](https://user-images.githubusercontent.com/108853290/183569814-e04dc5a8-dc23-4a03-821f-f60d042959ec.png)  

그리고   
`@Configuration` 없이 생성된 `memberRepository` 세개의 인스턴스들에 대해서    
`@Bean`이 붙은 `memberRepository`객체는 스프링이 관리해주는 스프링 빈이고   
나머지 `MemberServiceImpl`, `OrderServiceImpl` 의 `memberRepository`는 스프링 빈이 아닌것이 된다.
  

--------------------------------
 
정리하자면  
* `@Configuration`내에서 `@Bean`이 붙은 메서드를 호출하면 해당 메서드에서 생성하려는 타입의 빈이 컨테이너 존재하는지 확인하는 프로세스가 추가된다.   
* `@Bean`만 사용해도 스프링 빈으로 등록은 되지만, 싱글톤 보장해주진 않는다.
