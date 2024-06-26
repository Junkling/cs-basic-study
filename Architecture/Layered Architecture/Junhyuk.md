# 아키텍쳐

## **클린 아키텍처의 레이어 구조**

---

![image](https://blog.kakaocdn.net/dn/bOh3Wo/btrXn7BTilz/TWwETWquQqz79ndGY9gVc0/img.jpg)


안쪽에 위치할수록 고수준 정책이며, 바깥쪽에 위치할 수록 저수준 정책을 의미합니다.

### 엔티티 (Entities)

의도에 따라 **도메인 계층**으로도 불리며, 엔티티 계층은 **하나 이상의 프로그램 간에 공유**될 수 있다는 가정하에 만드는 **수명이 긴 객체**입니다. (즉, 재사용의 가능성이 높다는 것을 인지하고 외부에 의해 변경될 가능성을 낮추어야 합니다.)

이곳에는 Enterprise 규모의 비즈니스 데이터를 포함하거나 핵심이 되는 비즈니스 규칙을 캡슐화합니다.

### 유즈케이스 (Use cases)

**애플리케이션 계층**으로도 불리며, 어플리케이션 규모의 비즈니스 규칙을 포함합니다.

이 레이어의 변경사항은 **엔티티에 영향을 미쳐서는 안 되며,**

**인프라 단의 DB나 UI, 라이브러리와 같은 외부요소에 의해 영향을 받지 않는다는 것**을 원칙으로 합니다.

이는 즉 해당 계층의 수정은 응용 프로그램의 동작에 영향을 미친다는 의미입니다.

### 인터페이스 어뎁터 (Interface Adapter)

어뎁터 계층은 DB나 Web, UI와 같은 **바깥 계층에서 사용하기 편리하도록,** **유즈케이스 또는 엔티티 계층에서 데이터를 변환하는 어뎁터의 집합**입니다.

흔히 **MVC**, **MVVM**과 같은 아키텍처를 포함하는 것이 이 영역으로 컨트롤러, 프레젠터, 게이트웨이 등이 속합니다.

### 프레임워크와 드라이버 (Frameworks & Drivers)

**인프라 계층**이라고도 불리며, 가장 외부에 있는 레이어로 DB, 웹 프레임워크와 같은 세부 정보(Details)를 나타내는 계층입니다.

이곳은 보통 글루 코드(Glue code)만 작성하며, **시간이 지남에 따라 구성이 변경될 수 있으므로** 엔티티 계층(또는 도메인 계층)에 **추상화(abstracted)**하여 도메인 계층에 영향을 주지 않고 인터페이스를 수정하고 업데이트할 수 있습니다.

### 아키텍쳐를 왜 알아야할까

소프트웨어 프로젝트에서 아키텍처 설계는 시스템의 구조를 정의하고, 컴포넌트 간의 상호작용 방식을 결정하기 때문에 잘 설계된 아키텍처일수록 유지보수가 쉽고, 확장성이 높으며, 시스템의 성능을 최적화할 수 있다.

## 레이어드 아키텍쳐

아키텍쳐를 계층을 기준으로 나누어 구성하는 방식이다.

**예시**

> **Controller** -  **Service** - **Repository** - **Entity** - **Dto** / **Config** - **Excpetion** - **Aop**
> 

**장점** : 

- 계층에 대한 구분이 쉬워지고 레이어 간의 책임을 분리하여 구조화 할 수 있다.
- 계층별로 나뉘어 있기 때문에 흐름에 대한 판단이 빠르게 가능해진다.

**단점** :

- 계층에 대한 구분은 쉽지만 도메인을 기준으로 접근하였을 때는 구분이 어려워진다.
    - 계층별로 나뉘어 있기 때문에 특정 도메인의 흐름을 판단할 때 여러 패키지를 찾아다녀야한다.
- 계층을 기준으로 나뉜 아키텍쳐임으로 패키지간 의존관계가 많아진다.
    - 예를 들어 Cotroller - Service - Repository - Entity 와 같이 서로 다른 패키지의 클래스를 의존해야하며 이로 인해 확장과 유지보수에 불리하다.
    - 서비스가 거대해짐으로 인해 어플리케이션을 도메인 위주로 분리해야한다고 가정하였을 때 계층에 따라 분리되어 있는 아키텍쳐는 구조를 변경하기에 매우 불리하며 특정 계층의 변화가 있을 때 여러 계층에서 이를 의존함으로 의존성에 있는 모든 계층에 대한 코드 수정이 불가피하다.
   
## 모듈형 아키텍쳐

모듈(도메인)을 기반으로 구성되는 아키텍쳐이며 레이어드 아키텍쳐와 비교하였을 때 마이크로서비스로의 확장에 매우 유리한 조건을 가지고 있다.

**장점**

- **모듈화로 관리 편의성** : 기능별로 모듈화되어 관리가 용이하고 도메인 위주로 기능을 확장시키기 편리하다.
- **단순한 배포 및 운영**: 모노리식의 장점을 유지하면서도 모듈화된 구조를 가진진다.
- **마이그레이션**: 필요에 따라 모듈을 마이크로서비스로 분리할 수 있다.
- **협업** : 도메인 기준으로 나뉘어진 협업 시 관리가 용이함

**단점**:

- **모듈 간의 강한 연결성**: 모듈 간의 의존성이 존재함으로 종속성에 대한 관리 필요.
- **규모에 따른 복잡도 증가**: 애플리케이션가 커짐에 따라서 타 도메인 의존성이 복잡해지는 등 유지보수가 어려워짐.
- **기술스택**: 특정 모듈에는 필요 없는 기술 스택임에도 전체 애플리케이션이 동일한 기술 스택을 사용해야함.