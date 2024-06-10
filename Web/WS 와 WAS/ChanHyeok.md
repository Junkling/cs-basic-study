### Web Server VS WAS
<br></br>
**Static Pages 와 Dynamic Pages**

**Static Pages**

- Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
- 항상 동일한 페이지를 반환한다.
- Ex) image, html, css, javascript 파일과 같이 컴퓨터에 저장되어 있는 파일들

**Dynamic Pages**

- 인자의 내용에 맞게 동적인 contents를 반환한다.
- 즉, 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물 * Servlet: WAS 위에서 돌아가는 Java Program
- 개발자는 Servlet에 doGet()을 구현한다.<br></br>

**Web Server**

![Untitled (5)](https://github.com/5dotseven/cs-basic-study/assets/118906074/03de95c0-134b-4299-be82-cc61b35396a4)

- 정의
    - 클라이언트로부터 HTTP 요청을 받아들이고 정적 컨텐츠를 제공하는 서버
    - WAS와 연동하는 역할
- 기능
    - 정적인 컨텐츠 요청시 -> 파일 시스템에서 직접 제공
    - 동적인 컨텐츠 요청시 -> 클라이언트의 요청을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, response)한다.
- Ex) Apache, WebtoB, Nginx, IIS(Windows 전용 Web 서버)

**WAS(Web Application Server)**

![Untitled (6)](https://github.com/5dotseven/cs-basic-study/assets/118906074/30eb0a2a-7498-4c8c-b545-bb4cb608eb7c)

- 정의
    - DB조회나 다양한 로직처리 등을 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server
    - 클라이언트가 입력한 url의 인자를 받게되면 데이터베이스에서 해당데이터를 조회를 하거나 또는 로직처리해서 반환
    - 정적과 다르게 사용자와 특정인자값이 바뀌면 컨텐츠 내용이 바뀝니다.
- 구조
    - Web Server + Web Container의 역할을 모두 할 수 있다.
        - Web Containe : JSP, Servlet을 실행시킬 수 있는 소프트웨어
        - JSP, Servlet을 구동할 수있는 환경을 제공하여 웹컨테이너 OR 서블릿 컨테이너라고 정의

          (현재 WAS의 웹 서버도 정적인 컨텐츠를 처리하는 데 성능상 큰 차이가 없다.)

- 기능
    - 프로그램 실행 환경과 데이터베이스 접속 기능을 제공하여 비즈니스 로직을 수행
    - WAS도 클라이언트로부터 HTTP요청을 받을 수 있어서 (대부분의 WAS는 Web Server 내장
    - 요청에 맞는 정적 컨텐츠를 제공할 수 있고, 동적컨텐츠를 제공
- Ex) Tomcat, JBoss, Jeus, Web Sphere 등<br></br>

**Web server와 WAS를 따로 사용하는 이유**

1. **빠른 페이지 노출 및 서버 성능 향상**
    1. WAS가 정적 + 동적 콘텐츠를 한 번에 제공할 수 있지만, 이 경우 정적 콘텐츠 처리로 부하 증가 → 동적 콘텐츠를 처리 지연이 발생한다.Web Server의 Cache 기능을 활용해 서버 응답 시간 단축서버 자원 효과적 분배<br></br>
2. **로드 밸런싱(load balancing) 활용**
    1. Web Server 하나에 여러 개의 WAS를 연결해 사용할 경우 로드 밸런싱을 활용해 트래픽 분산 및 fail over(문제가 발생한 was 대신 다른 was로 요청 전달 등의 방식을 활용)가 가능하다.<br></br>
3. **보안 강화**
    1. reverse proxy server와 같이 WAS 앞에서 Web server가 먼저 클라이언트의 요청을 받으므로 WAS 에 직접적인 접근 차단 및 요청 필터링이 가능하다.<br></br>
4. **유지보수 및 확장의 용이성**
    1. Web Server와 WAS가 분리되어 있어 각 구성 요소를 유지보수하기 쉽고 독립적으로 확장하기 쉽다<br></br>

**Spring boot 사용시 NginX(웹서버)를 별도 사용하는 이유**

- Spring Boot는 내장된 Tomcat, Jetty, Undertow 등의 서블릿 컨테이너를 사용하여 웹 애플리케이션 실행 가능
- 이러한 서블릿 컨테이너는 개발 및 테스트 목적으로는 충분하지만, 운영 환경에서는 다음과 같은 이유로 웹 서버인 Nginx 등의 별도의 웹 서버와 함께 사용하는 것이 일반적.<br></br>

**1. 정적 파일 처리**

- Spring Boot는 정적 파일을 처리하는데 있어서 웹 서버에 비해 불리한 성능을 가집니다.
- 정적 파일을 서블릿 컨테이너로 처리하면, 스프링 애플리케이션의 자원을 낭비할 수 있으며, 부하도 증가할 수 있습니다.
- 따라서, 정적 파일들은 웹 서버에서 처리하여 성능을 향상시키는 것이 좋습니다.<br></br>

**2. SSL 인증서 및 암호화**

- HTTPS를 사용하여 통신할 경우, SSL 인증서 및 암호화 설정이 필요합니다.
- 이러한 설정은 서블릿 컨테이너보다는 웹 서버에서 수행하는 것이 효율적이며, 보안상의 이유로도 웹 서버를 사용하는 것이 좋습니다.<br></br>

**3. 로드 밸런싱**

- 대규모 서비스의 경우, 여러 대의 서버를 이용하여 로드 밸런싱을 수행하는 것이 일반적입니다.
- 이 경우, 로드 밸런서 역할을 하는 웹 서버를 별도로 두고,WAS가 처리해야 할 요청을 여러WAS로 분산처리 할 수 있습니다.<br></br>

**4. 성능 향상**

- 웹 서버는 정적 콘텐츠를 캐시하거나 압축 등의 최적화 기능을 제공하여 성능을 향상시킬 수 있습니다. 이러한 기능을 사용할 수 있으며, 서블릿 컨테이너보다 빠른 속도로 응답할 수 있습니다.
- 따라서, 운영 환경에서는 보안, 성능, 확장성 등을 고려하여, Spring Boot 애플리케이션과 함께 웹 서버를 사용하는 것이 일반적입니다.<br></br>

**WS(Web Server) VS WAS(Web Application Server) 요약**

- 주된 차이점은 역할과 처리하는 내용이다. WAS는 웹 애플리케이션의 비즈니스 로직과 데이터처리를 담당하여 WS는 정적인 웹 리소스를 서비스한다.
- WAS는 웹 서버의 기능도 가지고 있을 수 있으나, 보다 복잡하고 고급 기능을 제공하는 애플리케이션 서버이다.
- 웹 서버는 주로 리버스 프록시로 동작하여 요청을 WAS로 전달하거나 로드 밸런싱, 캐싱 등의 역할을 수행한다.
- WAS와 WS는 종종 함꼐 사용되어 웹 애플리케이션을 제공하는데 필요한 기능을 모두 제공한다.<br></br><br></br><br></br>

출처

- [https://velog.io/@wonizizi99/CS-웹-서버와-WAS-차이#3-2-️-wasweb-application-server](https://velog.io/@wonizizi99/CS-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%99%80-WAS-%EC%B0%A8%EC%9D%B4#3-2-%EF%B8%8F-wasweb-application-server)
- https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html