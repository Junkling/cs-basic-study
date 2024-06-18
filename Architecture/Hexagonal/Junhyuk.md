## 헥사고날 아키텍쳐

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F0d3ea0ec-c76e-42ee-8076-6d97f7e41c01%2FUntitled.png?table=block&id=3a64e495-a616-4052-9639-6191f4d5e686&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

### 주요 컴포넌트

### Adapters

- 인프라와 어플리케이션을 연결하는 부분만 담당하는 구현체
- 사용자의 요청을 받을 때 = driving adapter (Controller , Handler)
- 도메인 모델의 처리 시 = driven adapter
- 헥사고날 아키텍처에서는 사용자 인터페이스나 저장소(DB) 는 비지니스 로직으로부터 분리해야하는 요소이다.
- Adapter계층은 프레젠테이션 계층(사용자 인터페이스[in, out])에 대한 외부 통신을 담당한다.
    - in    : 사용자 인터페이스 (REST API, Grahphql 등)
    - out : 데이터베이스 관련 (RDB, Redis + ORM, JPA)

### Ports

- adapter의 기능 정의된 인터페이스 DI를 위한 추상화 계층

### Application Service

- adapter 주입을 받아 도메인 모델과 adapter를 적절하게 오케스트레이션 하고 비즈니스 로직을 담당하는 계층

### Domain model

- 모든 엔티티의 변경은 도메인 계층에서만 이루어지며 서비스계층에서 도메인의 기능을 호출하여 비즈니스 로직을 수행함
- 도메인 계층은 어떤 의존성도 없어야 한다.
    - 단 레포지토리 계층은 포트를 이용해 adapter를 주입받아서 사용한다. ⇒ DB를 저장하기 위해서 리파지토리 계틍을 의존해야하기 때문에 Port를 통해 의존성을 주입 받는다.
    

**장점** :

**명확한 관심사 분리**

- **어뎁터** : 어플리케이션이 외부의 연결과 문제가 생겼을 때 확인
- **포트** : 어뎁터에 대한 추상화 인터페이스
- **서비스** : 처리 중 EventBridge에 이벤트를 보내거나 트레이스 로그를 남기는 등
- **도메인** : 비즈니스 로직에 문제가 있을 시 확인

**테스트 간편화**

- **어뎁터** : 유효성 검증과 서비스 호출만 테스트
- **포트** : 어뎁터의 연결 테스트
- **서비스** : 서비스 계층과 도메인 계층의 연결 테스트
- **도메인** : 순수한 비즈니스 로직 케이스 테스트 , 비즈니스 로직에 의존성이 없기 때문에 모킹을 할 필요도 없다.