# 데코레이터 패턴

Interface 구성의 프록시 패턴에서 프록시를 겹겹이 쌓는 패턴으로

인터페이스를 구현한 구현체를 다시 Interface로 다른 프록시에 넣어 겹겹이 쌓는다.

**데코레이터 예시)**

```java
@Slf4j
public class MessageDecorator implements Subject {
    private Subject subject;

    public MessageDecorator(Subject subject) {
        this.subject = subject;
    }

    @Override
    public String operation() {
        log.info("timeDeco 실행");
        long start = System.currentTimeMillis();
        String result = subject.operation();
        long end = System.currentTimeMillis();
        long resultTime = end - start;
        log.info("timeDeco 종료 resultTime ={}ms",resultTime);

        return result;
    }
}
```

```java
@Test
void deco2() {
    RealSubject realSubject = new RealSubject();
    Subject cacheProxy = new CacheProxy(realSubject);
    Subject timeDeco = new TimeDecorator(cacheProxy);
    ProxyPattenClient client = new ProxyPattenClient(timeDeco);
    client.execute();

// 데코레이션 패턴 결과/*
    timeDeco 실행
    CacheProxy 실행
    실제 객체 호출
    timeDeco 종료 resultTime =8ms
    */
}
```

AOP에서의 관점 분리는 이와 같은 패턴으로 되어 있으며 프록시 패턴으로 인해 관심사를 분리하고 역할을 나누었지만

많은 클래스를 작성해야 한다는 단점**(만일 subject만 아니라 다른 클래스도 proxy를 적용하려면 그 인터페이스를 implents 하는 클래스를 그만큼 작성해야 함)**이 있다.

이러한 단점을 동적 프록시 기술로 해결하였다.

동적 프록시 기술은 InvocationHandler 인터페이스를 상속하여 Object를 target으로 받고

target으로 받은 Object의 class 내부의 메서드를 호출하도록 하여 하나의 프록시 클래스에서 여러 타입의 객체(인터페이스를 상속받은 구현체)를 받을 수 있게 하였다.