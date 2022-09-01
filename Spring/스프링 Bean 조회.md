# 스프링 Bean 조회

### 1) 모든 빈 출력하기 ###
```java
public class ApplicationContextInfoTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("모든 빈 조회")
    void findAllBean() {
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);   //Object로 꺼내짐. 타입을 모르기 때문에!
            System.out.println("bean = " + beanDefinitionName + " object = " + bean);
        }
    }
}
```
  
* `.getBeanDefinitionNames()`를 통해 스프링 컨테이너에 등록된 모든 빈들을 조회할 수 있다.
> 참고 : [BeanDefinition - 스프링 빈 설정 메타 정보](https://github.com/MJeong00/TIL/blob/main/BeanDefinition%20-%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EB%B9%88%20%EC%84%A4%EC%A0%95%20%EB%A9%94%ED%83%80%20%EC%A0%95%EB%B3%B4.md)  



### 2) 빈 조회 ###
```java
public class ApplicationContextBasicFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    @Test
    @DisplayName("빈 이름으로 조회")
    void findBeanByName() {
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("이름 없이 타입으로만 조회")
    void findBeanByType() {
        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }
}

```

* `.getBean()` : 빈 이름으로 빈 객체를 조회한다.
  * `.getBean(빈이름, 타입)` 또는 `.getBean(타입)`   
  * 빈 이름을 생략 가능하다.   
  * 만약 조회 대상 스프링 빈이 없으면 예외가 발생한다. `NoSuchBeanDefinitionException: No bean named 'xxxxx' available`   

  
  
### 3) 동일한 타입 둘 이상 ###
![image](https://user-images.githubusercontent.com/108853290/182817271-cd3718c3-4d71-4802-a141-22634e871883.png)   
타입으로 조회시 같은 타입의 스프링 빈이 둘 이상이면 오류가 발생한다.    
이때는 **빈 이름을 지정**한다.
   
![image](https://user-images.githubusercontent.com/108853290/182817573-0833ab51-8403-4104-afdf-46c8f25ce85e.png)   

* `.getBeansOfType()` 을 사용하면 해당 타입의 모든 빈을 조회할 수 있다.
