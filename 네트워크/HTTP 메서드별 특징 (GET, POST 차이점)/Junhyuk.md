# GET 메서드와 POST 메서드

## HTTP 메서드의 특성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/552fe0dc-fdb3-4c62-979e-df2a2e235613/5970b2ce-5f17-4683-9ee1-05a6237f7f22/Untitled.png)

**안전** : 리소스를 변경하지 않는다. 

**멱등(idempotent)** : 몇번을 호출하더라도 결과가 같다. ⇒ **PUT**과 **POST/PATCH**의 주요 차이점

- 멱등은 여러번 요청되는 사이에 **외부 요인으로 인한 변경은 고려하지 않는다**
    - 여러번의 **GET**요청 사이에 **POST** or **PUT**으로 인한 리소스 변경이 있어 GET 요청의 응답값이 다르더라도 이는 **GET**이 **멱등하지 않은 이유**가 되진 않는다.
- 자동 복구 메커니즘을 적용할지에 중요한 기준
    - 만일 멱등 하지 않다면 retry를 하도록 설정하면 안됨!!

**캐시 가능** : 응답결과 리소스를 캐시해도 되는가

- GET, HEAD,POST,PATCH는 캐시 가능
- 실제로는 GET,HEAD 정도 사용한다
    - POST,PATCH는 본문 내용까지 캐시 키로 고려해야함으로 구현이 어렵다

### Get

클라이언트에서 서버로 어떠한 리소스로부터 정보를 요청하기 위해 사용되는 메서드

데이터를 **읽거나**(Read), **검색**(Retrieve)할 때에 사용되는 method

GET은 요청을 전송할 때 URL 주소 끝에 파라미터로 포함되는 경우를 **URL파라미터(PathParam)**라 부르며, key ,value URL에 포함시키는 경우를 **쿼리 스트링(QueryString)이라고** 부른다.

> e.g.) www.example-url.com/resources?name1=xxx&name2=yyy
> 

### GET 요청에 대한 기타 참고 사항

- GET은 불필요한 요청을 제한하기 위해 요청이 캐시 될 수 있다.
- 파라미터에 내용이 노출되기 때문에 민감한 데이터를 다룰 때 GET 요청을 사용해서는 안 된다.
- GET 요청은 브라우저 기록에 남는다.
- GET 요청에는 데이터 길이에 대한 제한이 있다.
- Get 요청은 성공 시, 200(Ok) HTTP 응답 코드를 XML, JSON뿐만 아니라 여러 데이터(html, txt 등..), 여러 형식의 데이터와 함께 반환.
- GET 요청은 **idempotent 합니다.**

**idempotent= 멱등** 

동일한 연산을 여러 번 수행하더라도 동일한 결과가 나타나야 함.

GET은 **Idempotent**, POST는 **Non-idempotent**.

### Post

- POST method는 리소스를 생성/업데이트하기 위해 서버에 데이터를 보내는 데 사용.
- GET과 달리 전송해야 될 데이터를 HTTP 메시지의 Body에 담아서 전송.
- Body의 타입은 요청 헤더의 Content-Type에 요청 데이터의 타입을 표시 따라 결정.
- HTTP 메시지의 Body는 길이의 제한 없이 데이터를 전송할 수 있습니다.

### 사용 시점

- 리소스를 생성할 때
- 프로세스의 처리가 필요한 경우
    - 요청으로 인해 리소스가 생성되진 않지만 프로세스의 상태가 변화하는 경우
- 조회일지라도 Body 값으로 Json을 넘긴다던가  GET 메서드를 사용하기 어려운 경우

### Post 요청에 대한 기타 참고 사항

- POST 요청은 캐시 되지 않는다.
- POST 요청은 브라우저 기록에 남아 있지 않다.
- POST 요청을 북마크에 추가할 수 없다.
- POST 요청에는 데이터 길이에 대한 제한이 없다.
- Post 요청 중 자원 생성은 201(Created) HTTP 응답 코드를 반환한다.
- Post 요청은 **idempotent**하지 않다.

### Get과 Post의 차이점 정리

!https://blog.kakaocdn.net/dn/cFrp2h/btrGKSnYAQV/czqwff4JXfBNzzsffX6g40/img.png

### GET VS POST

| 캐시 | ⭕️ | ❌ |
| --- | --- | --- |
| 브라우저 기록 | ⭕️ | ❌ |
| 북마크 추가 | ⭕️ | ❌ |
| 데이터 길이 제한 | ⭕️ | ❌ |
| HTTP 응답 코드 | 200(Ok) | 201(Created) |
| 언제 주로 사용하는가? | 리소스 요청 | 리소스 생성 |
| 리소스 전달 방식 | 쿼리스트링 | HTTP Body |
| idempotent | ⭕️ | ❌ |

## 이외 HTTP 메서드

### PUT

- 리소스가 있으면 대체 , 없으면 생성 ⇒ 곧 덮어쓰기
- **POST와의 차이점**:  리소스의 위치를 알고 URL을 지정한다.
- 멱등(idempotent)하다 ⇒ 같은 요청을 여러번 호출해도 결과 값이 같다.(덮어쓰기 때문)

### PATCH

- 리소스의 위치를 알고 URL을 지정하는 점에선 **PUT**과 동일
- 요청을 기반으로 리소스의 부분을 대체한다는 점에서 PUT과 차이점

### DELETE

- 리소스를 제거

### 예상 질문

- PUT과 PATCH의 차이점을 설명해주세요.
    - 리소스를 대체하는가 요청을 기반으로 수정하는가의 차이점 PUT은 멱등하고 PATCH는 멱등하지 않음
- 멱등이란 무엇입니까.
    - 요청이 여러번 있을 때 결과값이 동일한가??
    - 자동 복구(Retry)의 기준이 된다.
- POST/PATCH에서 캐싱을 한다면 어떻게 해야 할까요
    - 구현이 어렵지만, POST,PATCH는 본문 내용까지 캐시 키로 고려해야 한다.
