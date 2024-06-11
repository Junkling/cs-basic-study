## 웹 서버

**웹 서버**(Web server)는 다음의 두 가지 뜻 가운데 하나이다.

1. 웹 서버: 웹 브라우저와 같은 클라이언트로부터 HTTP HTTPS 요청을 받고, HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램
2. 웹 서버 (하드웨어): 위에 언급한 기능을 제공하는 컴퓨터 프로그램을 실행하는 컴퓨터

웹 서버(web server)는 HTTP 또는 HTTPS를 통해 웹 브라우저에서 요청하는 [HTML 문서나 오브젝트(이미지 파일 등)을 전송해주는 서비스 프로그램을 말한다. 웹 서버 소프트웨어를 구동하는 하드웨어도 웹 서버라고 해서 혼동하는 경우가 간혹 있다

클라이언트(사용자)의 페이지 요청을 받아 **정적 컨텐츠를 제공하는 서버**

****정적 컨텐츠**

**단순 HTML 문서, CSS, javascript, 이미지, 파일 등 즉시 응답가능한 컨텐츠**

동적 컨테츠를 처리해야할 때는 WAS에게 해당 요청을 전달하고 , WAS에서 처리한 결과를 정적 컨텣츠에 담아서 클라이언트(사용자)에게 전달해주는 역할 한다.

이때 웹 서버가 정적 컨텐츠가 아닌 동적 컨텐츠를 요청받으면 WAS에게 해당 요청을 넘겨주고, WAS에서 처리한 결과를 클라이언트에게 전달하는 역할도 해준다. 

웹 서버 종류로 Apache, NginX 등이 있다.

## WAS

**웹 애플리케이션 서버**(Web Application Server, 약자 WAS)는 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 

인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해 주는 미들웨어(소프트웨어 엔진)로 볼 수 있다. 

웹 애플리케이션 서버는 동적 서버 콘텐츠를 수행하는 것으로 일반적인 웹 서버와 구별이 되며, 주로 데이터베이스 서버와 같이 수행이 된다. 

한국에서는 일반적으로 "WAS" 또는 "WAS S/W"로 통칭하고 있으며 공공기관에서는 "웹 응용 서버"로 사용되고, 영어권에서는 "Application Server" (약자 AS)로 불린다.

**데이터베이스의 조회나 다양한 로직 처리가 필요한 동적 컨텐츠를 제공**한다. 

WAS는 **JSP, Servlet 구동환경을 제공**해주며 웹 컨테이너 혹은 서블릿 컨테이너라고도 불린다.

- *** 대표적인 WAS 종류 : Tomcat**
- *** 웹 컨테이너 : 웹 서버가 보낸 JSP, PHP 등의 파일을 수행한 결과를 다시 웹 서버로 보내주는 역할을 함**

이러한 WAS는 웹 서버의 기능들을 구조적으로 분리하여 처리하고자 하는 목적 분리되었다. 

분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용되며 

비즈니스 로직 또한 수행한다.

이러한 WAS에는 Tomcat, JBoss, WebSphere 등이 있다.

## Static Pages와 Dynamic Pages

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F7f4d4b4d-ac69-47ab-ad4a-5d8c5c2298e7%2FUntitled.png?table=block&id=2bdf10f1-432f-4409-9980-b6207c18b47e&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

### Static Pages

Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
항상 동일한 페이지를 반환한다.
Ex) image, html, css, javascript 파일과 같이 컴퓨터에 저장되어 있는 파일들

### Dynamic Pages

인자의 내용에 맞게 동적인 contents를 반환한다. 즉, 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진다.

## Web Service Architecture

1. Client -> Web Server -> DB
2. Client -> WAS -> DB
3. Client -> Web Server -> WAS -> DB

## WAS 구조

WAS는 웹 서버의 역할을 할수 있으며 아주 오래전 웹서버는 실제로 WAS 서버가 웹페이지의 렌더링 까지 담당하였다.아래는 WAS의 구조이며 MVC 1 모델의 경우 JSP등이 컨트롤러의 기능을 하는 어플리케이션 기능을 담당하던 아키텍쳐이다.

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F72fc48a9-36d7-43bf-860d-839430baa1bc%2FUntitled.png?table=block&id=e293b500-5083-4eb8-ab22-d16b6916dd61&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

그렇다면 WAS만으로도 웹 서버를 구축할 수 있지 않나? 맞다 할 수 있다. 하지만 아래와 같은 이유로 두 기능을 분리하였을 때 이점이 있다.

1. **서버 부하 방지**
WAS와 웹 서버의 부하 정도가 다를 수 있다. 예를 들어 WAS의 로직에 부하가 있어 지연되는 상황에 위 와 같은 구조이면 웹 서버를 랜더링 하는 성능에도 문제가 일어난다.
1. **보안 강화**
SSL에 대한 암호화, 복호화 처리에 웹 서버를 사용 가능
1. **각 기능의 단일 책임 부여 가능**
    
    웹 서버는 정적 컨텐츠를 랜더링하는데 WAS는 해당 페이지에 필요한 로직 등을 수행하는데 집중할 수 있다.
    
2. **WAS 연결 가능**
하나의 웹 서버에 여러 WAS를 연동하고 로드밸런싱 하는 등 확장이 편리하다.
1. **다른 환경의 웹 어플리케이션 연동 가능**
위와 같은 이유로 여러 다른 환경에 있는 WAS를 웹페이지와 연동 할 수 있다. PHP, JAVA, Python, JS 등

## WEB과 WAS가 분리된 아키텍쳐

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2Fcc9d8437-aa32-41d6-8578-380ce010dce0%2FUntitled.png?table=block&id=ed6d1630-bdf6-4306-ab73-596b023256fc&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 Servlet에 대한 Thread를 생성한다. (Thread Pool 이용)
5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
6. 서블릿 뒤에 존재하는 서비스 로직을 수행한다.
7. 로직에 의한 결과 값을 HttpResponse 형태로 Web Server에 전달한다.
8. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.