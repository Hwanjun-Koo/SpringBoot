사실 `@Configuration`은 싱글톤을 위해 존재한다

AppConfig를 다시보면

```
@Configuration //설정 정보  
public class Appconfig { //전체 애플리케이션의 구성  
  
    //@Bean memberService -> new MemoryMemberRepository()  
    //@Bean orderService -> new MemoryMemberRepository()    @Bean //스프링 컨테이너에 등록  
    public MemberService memberService() { //회원 서비스 객체 생성  
        return new MemberServiceImpl(memberRepository()); //MemoryMemberRepository를 생성자를 통해 넣어줌(생성자 주입)  
    }  
  
    @Bean  
    public MemberRepository memberRepository() {//회원 저장소 객체 생성  
        return new MemoryMemberRepository(); //나중에 DB로 바꾸고 싶다면 여기만 바꾸면 됨  
    }  
  
    @Bean  
    public OrderService orderService() { //주문 서비스 객체 생성  
        return new OrderServiceImpl(memberRepository(), discountPolicy()); //생성자 주입  
    }  
  
//    public DiscountPolicy discountPolicy() { //할인 정책 객체 생성  
//        return new FixDiscountPolicy(); //나중에 할인 정책을 변경하고 싶다면 여기만 바꾸면 됨  
//    }  
    @Bean  
    public DiscountPolicy discountPolicy() { //할인 정책 객체 생성  
        return new RateDiscountPolicy(); //바뀐 할인 정책  
    }  
}
```

`MemoryMemberRepository`가 2번 호출되네? 싱글톤이 깨지는거 아닌가??

테스트 코드를 짜보자

```
public class ConfigurationSingletonTest {  
  
    @Test  
    void configurationTest() {  
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);  
  
        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);  
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);  
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);  
  
        // 참조값이 다른 것을 확인  
        MemberRepository memberRepository1 = memberService.getMemberRepository();  
        MemberRepository memberRepository2 = orderService.getMemberRepository();  
  
        System.out.println("memberService -> memberRepository1 = " + memberRepository1);  
        System.out.println("orderService -> memberRepository2 = " + memberRepository2);  
        System.out.println("memberRepository = " + memberRepository);  
    }  
}
```

![](https://i.imgur.com/5WlTS6K.png)

참조 인스턴스가 같네????

로그를 남겨보자.


```
@Configuration //설정 정보  
public class Appconfig { //전체 애플리케이션의 구성  
  
    //@Bean memberService -> new MemoryMemberRepository()  
    //@Bean orderService -> new MemoryMemberRepository()  
    @Bean //스프링 컨테이너에 등록  
    public MemberService memberService() { //회원 서비스 객체 생성  
        System.out.println("call Appconfig.memberService"); //확인용  
        return new MemberServiceImpl(memberRepository()); //MemoryMemberRepository를 생성자를 통해 넣어줌(생성자 주입)  
    }  
  
    @Bean  
    public MemberRepository memberRepository() {//회원 저장소 객체 생성  
        System.out.println("call Appconfig.memberRepository"); //확인용  
        return new MemoryMemberRepository(); //나중에 DB로 바꾸고 싶다면 여기만 바꾸면 됨  
    }  
  
    @Bean  
    public OrderService orderService() { //주문 서비스 객체 생성  
        System.out.println("call Appconfig.orderService"); //확인용  
        return new OrderServiceImpl(memberRepository(), discountPolicy()); //생성자 주입  
    }  
  
//    public DiscountPolicy discountPolicy() { //할인 정책 객체 생성  
//        return new FixDiscountPolicy(); //나중에 할인 정책을 변경하고 싶다면 여기만 바꾸면 됨  
//    }  
    @Bean  
    public DiscountPolicy discountPolicy() { //할인 정책 객체 생성  
        return new RateDiscountPolicy(); //바뀐 할인 정책  
    }  
}
```

![](https://i.imgur.com/CYtzLrY.png)
memberRepository가 1번밖에 호출이 안되네? 