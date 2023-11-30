스프링은 어떻게 다양한 설정 형식을 지원하는가? - BeanDefinition

여기서도 **역할과 구현의 분할** 이 나온다
스프링 컨테이너는 사실 BeanDefinition만 있으면 된다

`@Bean, <bean>`  하나 당 하나의 메타 정보가 생기는 것

## AnnotationConfigApplicationContext의 경우

ac는 `AnnotatedBeanDefinitionReader` 를 사용해 `AppConfig.class`를 읽어 BeanDefinition을 생성한다

## BeanDefinition이 그래서 뭐야

테스트

```
public class BeanDefinitionTest {  
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);  
  
    @Test  
    @DisplayName("빈 설정 메타 정보 확인")  
    void findApplicationBean() {  
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();  
  
        for (String beanDefinitionName : beanDefinitionNames) {  
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);  
  
            if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {  
                System.out.println("beanDefinitionName = " + beanDefinitionName + " beanDefinition = " + beanDefinition);  
            }  
        }  
    }  
}
```

BeanDefinition 정보 
- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig 
- factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService 
- Scope: 싱글톤(기본값) 
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연 처리 하는지 여부 
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명 
- DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명 
- Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용 하면 없음)

BeanDefinition을 직접 정의할 경우는 거의 없지만 가끔 스프링 코드에서 발견하면 떠올릴 정도만 되면 된다 ㅎㅎ