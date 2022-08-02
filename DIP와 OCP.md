# DIP와 OCP    
> 해당 예시는 김영한님의 인프런 강의 중 [스프링 핵심 원리]를 바탕으로 합니다.   
> 해당 예시는 spring의 의존성 주입 없이 순수 자바로된 예시입니다. 스프링을 사용한 의존성 주입은 후에! 

## DIP란?
### DIP (Dependency Inversion Principle) : 의존성 역전 원칙 ###
* 상위 모듈은 하위 모듈에 의존해서는 안된다
* 추상화는 구체화에 의존하면 안된다.

## OCP란?
### OCP (Open Close Principle) : 개방 폐쇠의 원직 ###
* 확장에는 열려 있고, 변경에는 닫혀 있다.
* 즉 기능 수정이 있을 때 해당 기능을 사용하는 코드에는 변경이 없도록 한다. 
---------------------------------------------------------------   
   
   
할인 정책이 정률할인과 정액할인 두가지의 방안이 있고 어떤 정책을 쓸지 미확정이라고 가정 했을 때   
**할인정책의 공통적인 역할과 틀을 interface로** 두고 비율할인과 정액할인을 구체 클래스로 작성한다.    
> **interface, 추상화를 활용함으로서 확장성이 용이해짐! -> 좋은 설계**   

이 때    
```java
public class OrderServiceImpl implements OrderService{
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    
    ...
    
}
```   
이런식으로 할인 정책을 가져오게 된다면 할인정책을 사용하는 서비스계층에서의 의존관계는   
![image](https://user-images.githubusercontent.com/108853290/182097253-45f76bbd-a628-41e6-95f8-6dc3080d9f9d.png)   
이런 그림이 되어버린다.   
즉, **추상화에도 의존하고 구체화에도 의존하고있는 상황이 되어 DIP를 위반**하게 되는것이다.   
또한 정액할인(FixDiscountPolicy)가 아닌 정률할인(RateDiscountPolicy)로 **할인정책이 바뀌게될 경우   
해당 할인을 사용하고 있는 service계층의 코드도 수정해야하기때문에 OCP도 위반**하게 된다.  

  ---------------------------------------------------------------------------   
  
이를 위해서는    
**추상에만 의존하도록 변경(인터페이스에만 의존)하여 DIP를 준수하며,  
후에 할인 기능 정책이 변경되었어도 이를 사용하고 있는 service계층에서의 코드의 변경이 없도록, 즉 OCP를 준수하도록 한다.**   
      
![image](https://user-images.githubusercontent.com/108853290/182101435-59359f8e-5650-4064-81b9-cdb50a8938d8.png)      

```java
public class OrderServiceImpl implements OrderService{
    private DiscountPolicy discountPolicy;
    
    ...
    
}
```    
구체 클래스를 없앴지만? 그럼 누군가 외부에서 구현객체를 직접 생성해주고 넣어줄 무언가가 필요하다!   
그 역할을 해줄 어플리케이션 연결에 관한 전반적인 설정파일을 AppConfig로 둔다.    
      
      
### AppConfig ; 애플리케이션 연결 설정파일 ###   
해당 클래스는 **애플리케이션의 전체 동작 방식을 구성하기 위해, 구현 객체를 생성하고, 연결하는 책임을 가지는것을 목적으로 둔 별도의 클래스**이다.   
> 그러므로 전반적인 설정에 관한 클래스는 최상위 패키지에 두는것이 용이!   

```java
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private MemoryMemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy() {
        return new FixDiscountPolicy();
    }


}
```
* 애플리케이션 실제 동작에 필요한 **구현 객체를 생성**한다.
* 해당 객체는 생성자를 통해서 주입(연결)해준다.   

```java
public class MemberServiceImpl implements MemberService {

    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    
   ...
}

```
Appconfig의 도입으로 service계층은 
* **구체 클래스에 의존하지 않으며**(DIP 준수)
* **의존관계에 대한 고민은 외부에 맡기고 기능 실행에만 집중**하게 된다.
* 중복을 제거하여 수정시 한부분만 고치면 된다.(구체 영역이 바뀌면 연결 구성을 담당하는 AppConfig만 고치면 된다.)   
* 전체 구성을 빠르게 파악할 수 있다.
   
   
> `memberServiceImpl`의 입장에서는 **의존관계를 마치 외부에서 주입해주는것과 같다고 해서 DI(Dependency Injection), 의존관계를 주입**해 준다고 한다.   
`memberServiceImpl`의 입장에서 추상화 객체에는 어떤 구체 클래스가 들어올지는 모르겠지만, 아무튼 틀에 맞춰 쓰여진 어떠한 구체클래스가 들어온다. A 구체클래스가 들어오던지, B 구체클래스가 들어오던지! 어떤게 들어올지는 모르겠지만 `memberServiceImpl`은 기능 실행에만 집중할 뿐이다.
