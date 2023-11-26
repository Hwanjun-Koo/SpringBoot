실제로 빈이 컨테이너에 등록됐는지 확인해보자

- 테스트 작성

```
public class ApplicationContextInfoTest {  
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);  
  
    @Test  
    @DisplayName("모든 빈 출력하기")  
    void findAllBean() {  
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();// 스프링에 등록된 모든 빈 이름을 조회한다.  
        for (String beanDefinitionName : beanDefinitionNames) {  
            Object bean = ac.getBean(beanDefinitionName);// 빈 이름으로 빈 객체(인스턴스)를 조회한다.  
            System.out.println("name = " + beanDefinitionName + " object = " + bean);  
        }  
    }  
}
```
- 결과
![](https://i.imgur.com/7ts2A88.png)

위에 4줄(org.으로 시작)은 스프링 내부에서 사용하기 위한 빈
appconfig부터 직접 넣어준 빈

**만약 스프링 자체 제작 빈 말고 내가 만든 빈만 보고 싶다면?**


```
@Test  
@DisplayName("애플리케이션 빈 출력하기")  
void findApplicationBean() {  
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();// 스프링에 등록된 모든 빈 이름을 조회한다.  
    for (String beanDefinitionName : beanDefinitionNames) {  
        BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);// 빈 이름으로 빈 객체(인스턴스)를 조회한다.  
        if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){// 직접 등록한 빈  
            Object bean = ac.getBean(beanDefinitionName);  
            System.out.println("name = " + beanDefinitionName + " object = " + bean);  
  
        }  
    }  
}
```
- `beanDefinition.getRole() == Beandefinition.ROLE_APPLICAITON`: 개발자가 직접 애플리케이션 제작을 위해 만든 빈인지 확인하는 동작
- 반대로 스프링 내부 동작을 위한 빈만 보려면 ROLE_INFRASTRUCTURE로 바꿔주면 됨

- 결과
![](https://i.imgur.com/bBhXjsz.png)
