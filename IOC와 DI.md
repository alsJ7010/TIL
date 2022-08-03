# IOC와 DI

## IOC (Inversion of Control) 제어의 역전
**클라이언트 구현 객체가 필요한 객체를 스스로 생성하고 연결하는것이 아닌,   
외부에서 관리하는것을 제어의 역전**이라 한다.
```java
public class OrderServiceImpl implements OrderService{
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    
    ...
}
```
위의 코드처럼 `serviceImple`에 필요한 객체를 직접 `new`로 생성하고 그 객체를 쓰는것은 IOC가 아닌 일반적인 흐름이다.   
한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다고 할 수 있다.    
[DIP와 OCP](https://github.com/MJeong00/TIL/blob/main/DIP%EC%99%80%20OCP.md)를 보면 AppConfig의 등장으로
`OrderServiceImpl`는 자신의 기능에 집중하고 프로그램의 제어 흐름은 `AppConfig`가 가져간다. (AppConfig의 책임이 전체적인 구성에 대한 객체의 생성과 연결이기 때문!)   
이 경우에는 **`OrderServiceImpl`에 필요한 객체를 직접 생성해서 쓰는것이 아닌 `AppConfig`, 즉 외부에서 생성하고 연결된것이다.**
      
또한    
## DI (Dependency Injection) 의존관계 주입
```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository;    //추상클래스
    private final DiscountPolicy discountPolicy;        //추상클래스

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    ...
}
```
위의 코드에서 `OrderServiceImpl`의 입장에서는 인터페이스에 의존한다는것은 알지만 어떤 구체 클래스가 들어올지는 모르는것이다.    
변수에 들어온 참조값을 가지고 묵묵히 자기 기능을 실행할 뿐이다.   


![image](https://user-images.githubusercontent.com/108853290/182585936-496a63c6-ad45-4b28-9578-1906425f4cda.png)   
   
클래스 다이어그램 **(정적인 클래스 의존관계)** 으로 보면 `OrderServiceImpl`는 실제 어떤 구체 클래스가 객체에 들어올지 알 수 없다.   
하지만 어플리케이션 실행 시점에서 생성된 객체 다이어그램 **(동적인 객체 인스턴스 의존 관계)** 에서는    

![image](https://user-images.githubusercontent.com/108853290/182586953-982ca10d-2df9-4227-a8f2-d35d9f65b104.png)   

저장소 중에서도 메모리 회원 저장소, 할인 정책 중에서도 정액 할인 정책을 쓴다는것을 알 수 있다.   

이처럼 애플리케이션 **실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서
클라이언트와 서버의 실제 의존관계가 연결 되는 것을 의존관계 주입**이라 한다.   
   
의존관계 주입을 사용하면 클라이언트 코드, 즉 해당 객체를 사용하는 `serviceImpl`에서는 코드를 변경하지 않고 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
( = 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.)
