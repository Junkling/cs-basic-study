### REST와 RESTful의 개념

- **REST(Representational State Transfer)**
    - **정의**
        - 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것
        - 자원을 주고받는 웹 상에서의 통신 체계에 있어서 범용적인 스타일을 규정한 아키텍쳐
    - **개념**
        - HTTP URI(Uniform Resource Identifier)를 통해 자원을 명시,
          HTTP Method(POST, GET, PUT, DELETE)로 해당 자원 (URI)에 대한 CRUD Operation을 적용하는 것
            - CRUD Operation

              CRUD Operation
              Create : 생성(POST)
              Read : 조회(GET)
              Update : 수정(PUT)
              Delete : 삭제(DELETE)
              HEAD: header 정보 조회(HEAD)

    - **구성요소**
        - 자원(Resource) : HTTP URI
        - 자원에 대한 행위(Verb) : HTTP Method
        - 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load
    - **특징**
        1. 균등한 인터페이스 (Uniform Interface)
            1. URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
            2. HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다. → 특정 언어나 기술에 종속되지 않는다.(JSON, XML)

        2. 무상태성 (Stateless)
            1. HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
            2. Client의 context를 Server에 저장하지 않는다.
               즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
            3. Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리
                1. 각 API 서버는 Client의 요청만을 단순 처리한다.
                2. 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
                3. 물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
                4. Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.

        3. 캐싱 가능 (Cacheable)
            1. 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
                1. 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
                2. HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
            2. 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
            3. 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.

        4. 자체 표현성 (Self-Descriptiveness)
            1. REST API의 자원명시 규칙 및 메소드는 그 자체로 의미를 지니기 때문에 어떠한 요청에 있어서 그 요청 자체로 어떤 것을 표현하는지 알아보기 쉽다. 물론 API를 규정한 각 서비스들이 문서를 제공하지만 이 특성에 따라서 요청하는 방식만으로 어떠한 의미인지 알 수 있어야 좋은 REST API라고 할 수 있다.

        5. 클라이언트-서버 구조 (Client-Server Architecture)
            1. 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.
                1. REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
                2. Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
            2. 서로 간 의존성이 줄어든다.

        6. 계층형 구조 (Layered System)
            1. Client는 REST API Server만 호출한다.
            2. REST Server는 다중 계층으로 구성될 수 있다.
                1. API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
                2. 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.
            3. PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.

    - 장단점
        - 장점
            - HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.
            - HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
            - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
            - Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
            - REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
            - 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
            - 서버와 클라이언트의 역할을 명확하게 분리한다.
        - 단점
            - 표준이 자체가 존재하지 않아 정의가 필요하다.
            - 사용할 수 있는 메소드가 4가지밖에 없다.
            - HTTP Method 형태가 제한적이다.
            - 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
            - 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)
    - REST API
        - 정의 : REST의 원리를 따르는 API
        - 설계 규칙
            1. **URI**는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.

            ```
            Bad Example http://khj93.com/Running/
            Good Example  http://khj93.com/run/
            ```

            1. 마지막에 슬래시 (/)를 포함하지 않는다.

            ```
            Bad Example http://khj93.com/test/
            Good Example  http://khj93.com/test
            ```

            1. 언더바 대신 하이폰을 사용한다.

            ```
            Bad Example http://khj93.com/test_blog
            Good Example  http://khj93.com/test-blog
            ```

            1. 파일확장자는 URI에 포함하지 않는다.

            ```
            Bad Example http://khj93.com/photo.jpg
            Good Example  http://khj93.com/photo
            ```

            1. 행위를 포함하지 않는다.

            ```
            Bad Example http://khj93.com/delete-post/1
            Good Example  http://khj93.com/post/1
            
            ```

    - RESTful
        - 정의
            - 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어
                - ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다.
            - RESTful은 REST를 REST답게 쓰기 위한 방법
              즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.
        - 목적
            - 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
            - RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

출처

- [https://velog.io/@rlaclgns321/REST-API란-REST-RESTful이란](https://velog.io/@rlaclgns321/REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
- https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html