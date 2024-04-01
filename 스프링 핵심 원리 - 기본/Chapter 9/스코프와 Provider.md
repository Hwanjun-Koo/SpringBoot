
```
@Controller  
@RequiredArgsConstructor  
public class LogDemoController {  
  
    private final LogDemoService logDemoService;  
    private final ObjectProvider<MyLogger> myLoggerProvider;  
  
    @RequestMapping("log-demo")  
    @ResponseBody //http 응답 바디에 데이터를 직접 넣어주겠다.  
    public String logDemo(HttpServletRequest request) {  
        String requestURL = request.getRequestURL().toString(); //요청 URL 가져오기  
        MyLogger myLogger = myLoggerProvider.getObject();  
          
        myLogger.setRequestURL(requestURL);  
        myLogger.log("controller test");  
        logDemoService.logic("testId");  
        return "OK";  
    }  
}

```
이렇게 변경 - 컨트롤러에서 빈 생성을 지연할 수 있다

```
@Service  
@RequiredArgsConstructor  
public class LogDemoService {  
  
    private final ObjectProvider<MyLogger> myLoggerProvider;  
  
    public void logic(String id) {  
        MyLogger myLogger = myLoggerProvider.getObject();  
        myLogger.log("service id = " + id);  
    }  
  
}
```

결과
![](https://i.imgur.com/0zMGq8V.png)

컨트롤러, 서비스에서 각각 호출해도 같은 요청이면 같은 스프링 빈 반환한다

