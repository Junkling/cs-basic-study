# CORS란
## CORS란?
    출처가 다른 자원들을 공유한다는 뜻으로, 추가 HTTP 헤더를 사용하여 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 자원에 접근할 수 있는 권한**을 부여하도록 **브라우저**에 알려주는 체제이다.

브라우저에서는 보안적인 이유로 **cross-origin** HTTP 요청들을 제한한다. 

그래서**cross-origin** 요청을 하려면 서버의 동의가 필요하다. 

만약 서버가 동의한다면 브라우저에서는 요청을 허락하고, 동의하지 않는다면 브라우저에서 거절

이러한 허락을 구하고 거절하는 메커니즘을 HTTP-header를 이용해서 가능한데, 이를 CORS(Cross-Origin Resource Sharing)라고 부른다.

# **CORS가 필요한 이유**

CORS가 없이 모든 곳에서 데이터를 요청할 수 있게 되면, 기존 사이트와 완전히 동일하게 동작하도록 하여 사용자가 로그인을 하도록 만들고, 로그인했던 세션을 탈취하여 악의적으로 정보를 추출하거나 다른 사람의 정보를 입력하는 등 공격을 할 수 있어 이러한 공격을 할 수 없도록 브라우저에서 보호하고, 필요한 경우에만 서버와 협의하여 요청할 수 있도록 하기 위해서 필요하다.

**CORS**(Cross-Origin Resource Sharing / 교차 출처 리소스 공유)**란?**

- **동일출처**란?
    
    → 아래 사진(url구조)에서 **Protocol, Host, Port** 3개가 같으면 **동일 출처**(origin)라고 한다.
    
    ![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2Fdb32427f-0a44-492d-8815-cfcf28b27d26%2FUntitled.png?table=block&id=700681a2-0d7d-4706-b826-5f05073717d6&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)
    
    ❓ **동일출처 예** (http://junhyuk.com:80)
    
    - [ ]  https://junhyuk.com
        - http, https 는 동일 주소가 아니다.
    - [x]  http://junhyuk.com (O)
    - [ ]  http://junhyuk.cors.com
        - host 정보가 다르다.
    - [ ]  http://junhyuk.com:8080
        - port정보가 다르다

+) **SOP**(same-origin policy / 동일 출처 정책)이란 **다른 출처**로부터 조회된 자원들의 **읽기 접근을 막아** **다른 출처 공격**(CSRF, Cross-Site Request Forgery)을 **예방**하는 **보안 방식**이다.

### 다른 출처 요청이 위험한 이유

![Untitled](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F5e092d31-7da6-4718-9445-f093f3b084ae%2FUntitled.png?table=block&id=e2efe1ff-453e-4fff-861a-070b8a68ed40&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

    
- CORS 접근제어 방법
    
    **1) 단순 요청**(Simple Request)
    
    - 바로 요청을 날린다.
    
    - **GET, POST, HEAD** 요청만 가능하다.
    
    - Accept, Accept-Language, Content-Language, Content-Type과 같은 **CORS 안전 리스트 헤더** 혹은 **User-Agent 헤더**만 허용한다.
    
    - **Content-Type**은 application/x-www-form-urlencoded, multipart/form-data, text/plain만 가능하다.
    
    ![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F3d821d0a-1aaf-4beb-a496-600da67723a7%2FUntitled.png?table=block&id=d99539b8-715a-473e-b9ed-64369a3b3da2&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

    
    **2) 프리플라이트 요청**(Preflight Request)
    
    - OPTIONS메서드로 HTTP **요청을 미리 보내 실제 요청이 전송할 수 있는 상태인지 확인**한다.
    - **request 헤더**에는 아래 정보가 포함된다.
        - origin(요청 출처)
        - access-control-request-method(실제 요청이 보낼 HTTP 메서드)
        - access-control-request-headers(실제 요청에 포함된 HEADER)
    - **response 헤더**에는 아래정보가 포함된다.
        - access-control-allow-origin(서버가 허용하는 출처)
        - access-control-allow-methods(서버가 허용하는 HTTP 메서드 리스트)
        - access-control-allow-headers(서버가 허용하는 HEADER 리스트)
        - access-control-max-age(프리 플라이트 요청의 응답을 캐시에 저장하는 시간)
        
        → 모든 요청에 preflight 요청과 실제 요청을 총 2번 보낸다면 **리소스 낭비**가 발생한다 → 때문에 브라우저는 preflight 응답을 **캐싱**을 해두고 다음 똑같은 요청을 보낼 때 preflight 캐싱 확인하고 사전요청 없이 바로 본요청을 보내게된다.
        
    
    ![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F14034af6-d214-4887-b4fa-5d84be55c517%2FUntitled.png?table=block&id=7562f0d2-c6ae-4596-a516-2b2c60d8c3de&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)
    
    **3) 인증정보 포함 요청, 신용 요청**(Credentialed Request)
    
    - **인증 관련 헤더를 포함**할 때 사용하는 요청이다. (쿠키, 인증 헤더, TLS클라이언트 인증서 등..)
    - 기본적으로 CORS 정책은 다른 출처 요청에 인증정보 포함을 허용하지 않지만, 요청에 **인증을 포함하는 플래그**나 **access-control-allow-credentials = true** 설정 시 가능하다.
    
    → **access-control-allow-credentials = true** 설정 시 보안상의 문제로 인 모든 origin의 접근을 여는 **access-control-allow-origin:*** 는 막히게 된다 때문에 특정 경로를 지정해 주어야 한다.
    

CORS 설정법

- **Spring**
    
    **1) 컨트롤러에 설정하기**
    
    ```java
    @CrossOrigins(origins = "http://example.com", maxAge = 3600)
    @RestController
    @RequestMapping("/test")
    public class TestController{
    	
    	@RequestMapping(method = RequestMethod.GET, path = "/{id}")
    	public String findById(@PathVariable Long id) {
    		//...
    	}
    	
    	@RequestMapping(method = RequestMethod.DELETE, path = "/{id}")
    	public void remove(@PathVariable Long id) {
    		//...
    	}
    }
    ```
    
    **2) configuration 파일 생성** (전역 설정)
    
    ```java
    package com.elice.cors.config;
    
    import org.springframework.context.annotation.Configuration;
    import org.springframework.web.servlet.config.annotation.CorsRegistry;
    import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
    
    @Configuration
    public class CorsConfig implements WebMvcConfigurer {
    	private final long MAX_AGE= 3600;
    	
    	//메서드 오버라이딩
    	@Override
    	public void addCorsMapping(CorsRegistry registry) {
    		registry.addMapping("/**")
    		.allowedOrigins("http://localhost:3000")
    		.allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS")
    		.allowedHeaders("*")
    		.allowCredentials(true)
    		.maxAge(MAX_AGE);
    	}
    }
    ```
    
- **node.js**
  
  cors 미들웨어 활용법
    
    ```jsx
    const express = require('express');
    const cors = require('cors');
    
    app.use(cors(
        {
            //credentials를 true로 둘 경우 origin을 지정해야 한다.(보안상)
            origin: 'http://localhost:3060',
            // 모든 origin에서 접근 가능하게 땐 아래 두방법
            /* origin: ture,
               origin: '*',
            */
            //CORS환경에서 프론트엔드로 쿠키를 전달하려면 true
            credentials: true,
            allowedHeaders: ['Customheader= aaaa']
            methods: ['GET','HEAD','PUT','PATCH','POST','DELETE']
        }
    ));
    ```