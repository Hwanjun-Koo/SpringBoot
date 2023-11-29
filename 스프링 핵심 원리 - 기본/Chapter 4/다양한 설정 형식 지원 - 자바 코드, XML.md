스프링 컨테이너는 다양한 형식의 설정 정보를 받아들일 수 있게 설계되어있다

- AnnotationConfigApplicationContext - 지금까지 한 것
- GenericXmlApplicationContext - XML 파일을 넘겨서 사용
- XxxApplicationContext

XML을 사용하면 컴파일 없이 빈 설정 정보를 변경할 수 있다

테스트 코드를 먼저 만들어보자

```
public class XmlAppContext {  
    @Test  
    void xmlAppContext() {  
        ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");  
        MemberService memberService = ac.getBean("memberService", MemberService.class);  
        assertThat(memberService).isInstanceOf(MemberService.class);  
    }  
  
}
```

appConfig.xml을 기반으로 테스트를 하니까 이것도 만들어주면 된다


```
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
    </bean>  
  
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>  
  
    <bean id="orderService" class="hello.core.order.OrderServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>  
    </bean>  
  
    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>  
</beans>
```

형식은 복잡해 보이지만 이전에 작성한 AppConfig 클래스랑 똑같은 내용이다

