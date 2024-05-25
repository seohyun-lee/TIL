## TIL - Spring Security

### 1. Spring Security
Spring Security는 인증, 인가(권한 부여) 및 일반적인 악용에 대한 보호를 위한 포괄적인 지원을 제공하는 프레임워크이다. 또한 사용을 단순화하기 위해 다른 라이브러리와의 통합을 제공한다.
#### Authentication(인증)
특정 리소스에 액세스하려는 사람의 신원을 확인하는 것 (ex: username, password 요구) 인증이 수행되면 신원을 알고 권한 부여를 수행할 수 있다.
#### Authorization(인가)
특정 지원에 대한 액세스 권한이 있는지 확인하고, 허가된 사람에게 권한을 허락하는 것.
- Request Based Authorization(요청 기반 인가): 요청을 기반으로 인증을 제공하는 것.
- Method Based Authorization(메소드 기반 인가): 메소드 호출을 기반으로 인증을 제공하는 것.
### 2. 서블릿 기반 애플리케이션에서의 Spring Security 아키텍처
#### Filter
Spring Security는 서블릿 필터를 기반으로 서블릿을 지원한다. 단일 HTTP 요청에 대해 일반적으로 핸들러 계층은 다음과 같이 구성된다. ("필터체인")
![image](https://github.com/seohyun-lee/TIL/assets/32611398/6c8066f5-7495-4f6c-9c6e-b761c42985ae)
- 클라이언트는 애플리케이션에 요청을 보내고, 컨테이너는 요청 URI의 경로를 기반으로,HttpServletRequest를 프로세싱할 Servlet과 Filter 인스턴스들을 포함하는 FilterChain을 생성한다.
- Spring MVC 애플리케이션에서 Servlet은 DispatcherServlet의 인스턴스이다. 최대로 단일 Servlet은 하나의 HttpServletRequest및 HttpServletResponse를 다룰 수 있다. 그러나 여러가지 작업을 하기 위해서는 여러 필터가 필요하다.
```
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
    // 애플리케이션의 나머지 부분을 호출하기 전에 작업 수행
    chain.doFilter(request, response); // 애플리케이션의 나머지 부분 호출
    // 애플리케이션의 나머지 부분을 호출한 후 작업 수행
}
```
- 필터Spring Security는 단일 물리적이지만 Filter내부 필터 체인에 처리를 위임한다. DelegatingFilterProxy...등등
- 이미 존재하는 필터 체인에 사용자 정의 필터 추가해 사용 가능, 커스텀 필터 제작 가능
## 레퍼런스
- https://spring.io/projects/spring-security
- https://docs.spring.io/spring-security/reference/index.html
- https://docs.spring.io/spring-security/reference/servlet/architecture.html
- https://spring.io/guides/topicals/spring-security-architecture
