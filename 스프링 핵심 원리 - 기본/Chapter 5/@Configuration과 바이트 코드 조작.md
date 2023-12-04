스프링 컨테이너는 싱글톤 레지스트리이므로, 빈의 싱글톤을 보장해줘야 한다.
스프링이 자바 코드까지 조작해주기는 어렵다.

그 비밀을 파헤쳐 보자

```
@Test  
void configurationDeep() {  
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);  
    //Appconfig도 스프링 빈으로 등록된다.  
    Appconfig bean = ac.getBean(Appconfig.class);  
  
    System.out.println("bean = " + bean.getClass());  
}
```

`bean = class hello.core.Appconfig$$SpringCGLIB$$0` 
원래는 `class hello.core.Appconfig` 라고 출력되어야 한다
Appconfig\$\$SpringCGLIB\$\$0 -> CGLIB가 내부적으로 만들어준 클래스  

스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고,  그 다른 클래스를 스프링 빈으로 등록한 것이다!  
이 임의의 다른 클래스가 바로 싱글톤이 보장되도록 해준다.

이 행위 @Configuration이 해준 것이다.

@Bean이 붙은 메서드마다 이미 존재하는 빈이라면 그 빈을 반환하고 존재하지 않는 빈이면 새로 만들어 빈으로 등록하고 반환하는 코드가 자동으로 만들어진 것이다

### @Configuration을 제거한다면??

memoryRepository가 3번 호출되고, 각기 다른 인스턴스를 참조하게된다
스프링 컨테이너의 관리를 받지도 못함

사실 @Autowired로도 해결 가능 - 나중에 

결론: 설정 정보엔 항상 @Configuration을 넣자!