# 프록시 패턴
프록시는 '대리인'이라는 의미로 실체 객체의 대리객체로 실체 객체에 대한 접근 이전에 필요한 행동을 취할 수 있게 만들고 구현체를 바로 할당하지 않아 메모리의 이점을 가져오며, 실체 객체를 드러나지 않게 하여 정보은닉의 역할도 수행하는 패턴이다.
또한 구현체가 아닌 인터페이스를 기반으로 대리인을 둠 으로 수정과 확장에 열려있는 구조이다.

즉, 대상 클래스가 민감한 정보를 가지고 있거나 인스턴스화 하기에 무겁거나 추가 기능을 가미하고 싶은데, 원본 객체를 수정할수 없는 상황일 때를 극복하기 위해서 사용한다.

### 프록시 패턴에 적용할 수 있는 기능

- **보안** : 프록시는 클라이언트가 작업을 수행할 수 있는 권한이 있는지 확인하고 검사 결과가 긍정적인 경우에만 요청을 대상으로 전달한다.

- **캐싱** : 프록시가 내부 캐시를 유지하도록 구성하여 데이터가 프록시에 아직 존재한다면 대상에서 작업이 실행되지 않도록 할 수 있다.

- **유효성 검사** : 프록시가 입력을 구현체에 전달하기 전에 요청에 대한 유효성을 검사할 수 있다.
지연 초기화(Lazy Initialization) : 대상의 생성 비용이 비싸다면 프록시는 그것을 필요로 할때까지 연기할 수 있다.

- **로깅** : 프록시는 메소드 호출과 상대 매개 변수를 인터셉트하고 프록시에서 이를 기록할 수 있다.

- **원격 객체** : 프록시는 원격 위치에 있는 객체를 가져와서 로컬처럼 보이게 할 수 있다.

![image](https://blog.kakaocdn.net/dn/lvKQl/btsC4wNgyWF/bDd5K6wokV2TF8KGVP8kn1/img.png)

### 예제 코드

**구현체 예시)**

```java
@Slf4j
public class ConcreteLogic {
    public String operation() {
        log.info("concrete 로직 실행");
        return "data";
    }
}
```

```java
@Slf4j
public class TimeProxy extends ConcreteLogic {
    private ConcreteLogic concreteLogic;

    public TimeProxy(ConcreteLogic concreteLogic) {
        this.concreteLogic = concreteLogic;
    }

    @Override
    public String operation() {
        log.info("TimeDeco 실행");
        long start = System.currentTimeMillis();
        String result = concreteLogic.operation();
        long end = System.currentTimeMillis();
        long resultTime = end - start;
        log.info("timeDeco 종료 resultTime ={}ms",resultTime);

        return result;
    }
}
```

```java
public class ConcreteClient {
    private ConcreteLogic concreteLogic;

    public ConcreteClient(ConcreteLogic concreteLogic) {
        this.concreteLogic = concreteLogic;
    }

    public void execute() {
        concreteLogic.operation();
    }
}
```

```java
public class ConcreteProxyTest {
    @Test
    void noProxy() {
        ConcreteLogic concreteLogic = new ConcreteLogic();
        ConcreteClient concreteClient = new ConcreteClient(concreteLogic);
        concreteClient.execute();
    }

// noProxy 결과 ->// concrete 로직 실행

    @Test
    void addProxy() {
        ConcreteLogic concreteLogic = new ConcreteLogic();
        TimeProxy timeProxy = new TimeProxy(concreteLogic);
        ConcreteClient concreteClient = new ConcreteClient(timeProxy);
        concreteClient.execute();
    }
//addProxy 결과 ->/*
    TimeDeco 실행
    concrete 로직 실행
    timeDeco 종료 resultTime =1ms
    */
}
```

구현체의 경우 구현체 클래스를 직접 상속 (extends) 받아서 겉에 proxy를 감싸는 방법이다.

**인터페이스 예시)**

```java
public interface Subject {
    public String operation();
}
```

```java
@Slf4j
public class RealSubject implements Subject{
    @Override
    public String operation() {
        log.info("실제 객체 호출");
        sleep(1000);
        return "data";
    }

    private void sleep(int i) {
        try {
            Thread.sleep(i);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
@Slf4j
public class CacheProxy implements Subject {

    private Subject target;
    private String cacheValue;

    public CacheProxy(Subject target) {
        this.target = target;
    }

    @Override
    public String operation() {
        log.info("프록시 호출");
        if (cacheValue == null) {
            cacheValue = target.operation();
        }
        return cacheValue;
    }
}
```

```cpp
public class ProxyPattenClient {
    private Subject subject;

    public ProxyPattenClient(Subject subject) {
        this.subject = subject;
    }

    public void execute() {
        subject.operation();
    }
}
```

```java
public class ProxyPattenTest {
    @Test
    void noProxyTest() {
        RealSubject realSubject = new RealSubject();
        ProxyPattenClient client = new ProxyPattenClient(realSubject);
        client.execute();
        client.execute();
        client.execute();
    }
//noProxy 결과 (캐시에 저장되지 않고 계속 호출)/*
    실제 객체 호출
    실제 객체 호출
    실제 객체 호출
    */@Test
    void proxyTest() {
        RealSubject realSubject = new RealSubject();
        CacheProxy cacheProxy = new CacheProxy(realSubject);
        ProxyPattenClient client = new ProxyPattenClient(cacheProxy);
        client.execute();
        client.execute();
        client.execute();
    }
//Proxy 결과 (캐시에 저장되어 실제 객체까지 가지 않고 프록시에서 찾아옴)/*
    프록시 호출
    실제 객체 호출
    프록시 호출
    프록시 호출
    */
}
```