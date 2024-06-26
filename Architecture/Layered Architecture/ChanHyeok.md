### **Software Architecture**

- 모든 소프트웨어 시스템의 기본 구조를 말하며 시스템이 제대로 기능하고 작동하도록 하는 모든 측면을 말한다.
- 소프트웨어 시스템에서의 아키텍처는 물리적 설계가 아닌 **구성 요소의 설계**, **구성 요소 간의 관계**, **사용자 상호 작용 및 시스템에 대한 사용자의 요구**를 포함한다.
- Ex) Microservices, client-server, layered architecture 등의 구조들이 있다.

### **Layered Architecture**

![image (2)](https://github.com/5dotseven/cs-basic-study/assets/118906074/e91256d7-af4e-457d-a3f2-a8ab0899dbf0)
- **정의**
    - 소프트웨어 개발에서 일반적으로 가장 많이 사용되는 아키텍처이다.
    - Layer의 수에 따라 N Layered Architecture라고 불려지는데 **단일 소프트웨어 단위**로 **함께 기능**하는 여러 **개별 수평 Layer**로 구성된 아키텍처 패턴이다.
    - 즉, **각 Layer**은 애플리케이션 내에서의 **특정 역할과 관심사 별로 구분**되는 것이다.
  <br></br>
- **특징**
    - Layer1에서 Layer3에 접근하기 위해선 Layer2를 통해야한다는 것이다. 즉, **바로 아래 레이어로만 접근이 가능**
    - 위에서 말했던 관심사의 분리를 통한 격리 Layer라는 특징인데 이는 **하나의 Layer의 변경 사항은 변경된 특정 레이어로 격리된다는 것 →**종속성 저하
    - 응용 프로그램의 크기와 복잡성에 따라 3~5개 이상의 레이어로 구분된 구조들을 사용한다.<br></br>
- **장단점**<br></br>
    - **장점**
        - **관심사의 분리** - 각 개별 구성 요소의 단일 책임을 보장. ( 종속성 저하 )
        - **개발하기 쉬움** - 잘 알려져있고 구현하기 복잡한 패턴이 아니다. 대부분의 애플리케이션, 프로젝트에서 구현할 아키텍처 패턴이 확실치 않은 경우 좋은 출발점이다.
        - **테스트가 쉬움** - 모든 Layer가 개별적으로 단위 테스트로 커버될 수 있고 특정 Layer에 속한 구성요소도 분리되어 있어 개별적 테스트가 가능하다.
        - **격리** - 각 Layer가 다른 Layer와 독립적이어서 변경 사항이 다른 Layer로 영향을 끼치지 않는다.<br></br>
    - **단점**
        - **확장성** - 애플리케이션 복잡도가 증가하고 프로젝트에 더 많은 기능을 추가해야하는 경우 확장하는데 비용이 크다. (모놀리식 구현 경향)
        - **상호 의존성** - 하나의 계층이 데이터 수신을 위해선 상위 Layer에 의존하기에 상호 의존성이 존재한다.
        - **배포** - 특정 Layer에 대한 변경은 전체 시스템을 재배포해야 함을 의미한다. ( 큰 Application의 경우 더 문제가 된다. )
        - **성능** - 비즈니스 요청을 이행하기 위해 Architecture의 여러 Layer를 거쳐야 하는 비효율성으로 인해 고성능 Appication에 적합하지 않음. (병렬처리가 불가능)

### **3 Layered Architecture**

![image (3)](https://github.com/5dotseven/cs-basic-study/assets/118906074/669e6768-8619-457b-9f4f-3960744c6d97)
- 애플리케이션을 3개의 **논리적** 컴퓨팅 계층으로 구성하는 소프트웨어 애플리케이션이다.
- 2 Layered Architecture를 극복하기 위해 탄생했고 사용자 인터페이스 환경과 데이터 베이스 관리 환경 사이의 중간층이 추가된 구조이다.
- 중간층인 **Application Layer**에서는 **트랜잭션 처리/모니터, 메시지 서버, 응용 서버** 등 다양한 방법으로 구축될 수 있다.

1. **Presentation Layer**
- 소프트웨어 시스템과 사용자 상호 작용을 담당
- 주요 목적은 **정보를 표시하고 사용자로부터 정보를 수집하는 것,** 사용자 데이터를 가져와서 Application Layer로 전달하여 처리하는데 사용된다.
- EX) 웹 브라우저, 데스크탑 애플리케이션 또는 GUI에서 실행

1. **Business Layer**( **Service Layer)**
- 애플리케이션이 제공하는 기능을 정의하고, 세부 작업을 수행한다.
- Presentation Layer에서 **수집된 정보를 비즈니스 로직을 사용하여 처리**하는데 때로는 Data Access Layer의 다른 정보들과 비교해서 처리한다. (애플리케이션의 모든 비즈니스 로직에 따라 데이터를 처리함.)
- Data Access Layer의 데이터를 추가, 삭제 또는 수정할 수 있다.
- 스프링에서는 **서비스 계층(Service Layer)**이라고 부르기도 한다.

3. **Data Access Layer**

- 데이터베이스에 직접 접근하는 모든 작업을 수행한다.

출처

- https://gnuoyus.tistory.com/70