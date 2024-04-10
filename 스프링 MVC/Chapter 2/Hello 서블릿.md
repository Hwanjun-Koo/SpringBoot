# 참고

서블릿은 톰캣 같은 WAS를 직접 설치하고, 그 위에 서블릿 코드를 클래스 파일로 빌드해서 올린 다음, 톰캣 서버를 실행하는 것.
스프링 부트는 톰캣 서버를 내장하고 있어, 별도의 설치 없이 서블릿 코드 실행

# 스프링 부트 서블릿 환경 구성

- @ServletComponentScan: 서블릿 코드를 자동으로 스캔

```
@WebServlet(name = "helloServlet", urlPatterns = "/hello")  
public class HelloServlet extends HttpServlet {  
  
    @Override  
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        System.out.println("HelloServlet.service");  
    }  
}
```
- `@WebServlet`: 서블릿 어노테이션
	- name: 서블릿 이름
	- urlPatterns: URL 매핑
	- HTTP 요청을 통해 매핑된 URL이 호출되면, 서블릿 컨테이너가 메서드를 실행 
위와 같이 작성하고 브라우저에서 localhost:8080/hello를 입력하면 "White Label Error" 대신 빈 화면이 뜬다
![](https://i.imgur.com/INnXjFd.png)

![](https://i.imgur.com/sajmKZY.png)
메서드 호출이 잘 된 모습

이유: 아무것도 없는 Response 객체를 브라우저로 보냈기 때문!
![](https://i.imgur.com/O62Vwy5.png)


# 파라미터 불러오기
```
@Override  
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
  
    System.out.println("HelloServlet.service");  
    System.out.println("request = " + request);  
    System.out.println("response = " + response);  
  
    String username = request.getParameter("username");  
    System.out.println("username = " + username);  
}
```

위와 같이 수정해주면 파라미터로 뭐가 들어왔는지 알 수 있다
![](https://i.imgur.com/owO6jWu.png)
![](https://i.imgur.com/yabBljd.png)

# 응답 보내기

```
@Override  
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
  
    System.out.println("HelloServlet.service");  
    System.out.println("request = " + request);  
    System.out.println("response = " + response);  
  
    String username = request.getParameter("username");  
    System.out.println("username = " + username);  
  
    response.setContentType("text/plain");  
    response.setCharacterEncoding("utf-8");  
    response.getWriter().write("hello " + username);  
}
```

이렇게 보내주면
![](https://i.imgur.com/X2eiVfh.png)
반환 값이 생긴다





