# BeanDefinition - 스프링 빈 설정 메타 정보
> 해당 정보는 김영한 님의 인프런_[스프링 핵심 원리]를 참고로 합니다.   
  
    
    
* **`BeanDefinition` 은 속성 값, 생성자 인수 값 및 구체적인 구현에서 제공하는 
추가 정보가 있는 빈 인스턴스를 설명**한다.

* **스프링 컨테이너는 이 메타정보를 기반으로 인스턴스(빈)를 생성**할 수 있다.
* `@Bean , <bean>` 당 각각 하나씩 메타 정보가 생성된다.

#### BeanDefinition 정보 ####
* BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
* factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
* factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
* Scope: 싱글톤(기본값)
* lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한
생성을 지연처리 하는지 여부
* InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
* DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
* Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리
역할의 빈을 사용하면 없음)


------------------------------------------------------
## 스프링의 다양한 설정 파일 지원 이유 ##

스프링이 .class던지 .xml 등의 설정 파일처럼 다양한 설정 형식을 어떻게 지원하는걸까?   
바로 **`BeanDefinition`의 추상화** 덕분이다.   
`BeanDefition`은 인터페이스이며, 덕분에   
설정 파일로 .class던지 .xml던지 혹은 다른 형식의 파일이 들어와도 똑같이 `BeanDefition`을 만들면
스프링 컨테이너는 `BeanDefition`을 사용할 뿐이다.  -> 추상화에 의존!
  
  
![image](https://user-images.githubusercontent.com/108853290/183241672-09fe5149-6fad-4876-b658-6caefb0bf459.png)
   
각각의 파일 형식에 맞춰 `BeanDefition`을 생성해주는 클래스가 있다.   
설정정보를 바탕으로 그에 맞는 클래스로 `BeanDefition`을 생성해주면 스프링 컨테이너는 이를 바탕으로 빈을 생성ㅎ나다.

#### 정리: 스프링이 다양한 형태의 설정 정보를 `BeanDefinition`으로 추상화해서 사용하는것! ####
