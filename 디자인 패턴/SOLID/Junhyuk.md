## 객체지향 설계원칙

흔히 SOLID 라고 부르는 **5가지 설계원칙이 존재**한다. 

### SRP (Single Responsibility) 단일 책임 원칙

- 클래스는 **단 한개의 책임**을 가져야 함
- 클래스를 **변경하는 이유는 단 하나**여야 함
- 이를 지키지 않으면, 한 책임의 변경에 의해 **다른 책임과 관련된 코드에 영향을 미칠 수** 있음→ 이렇게 되면 **유지보수가 매우 비효율적**

### OCP (Open-Closed) 개방-폐쇄 원칙

- **확장에는 열려있어야 하고, 변경에는 닫혀 있어야 함**
- 즉, **기존의 코드를 변경하지 않고** 기능을 **수정하거나 추가**할 수 있도록 설계해야 함
- 이를 지키지 않으면 `instanceof` 와 같은 연산자를 사용하거나, 다운 캐스팅 발생

**어떤 모듈의 기능을 하나 수정**할 때, 그 모듈을 이용하는 **다른 모듈들 역시 줄줄이 고쳐야** 한다면 유지보수가 **복잡할 것**이다. 따라서 개방 폐쇄 원칙을 잘 적용하여 기존 코드를 변경하지 않아도 기능을 새롭게 만들거나 변경할 수 있도록 해야 한다.

**그렇지 않으면 객체지향 프로그래밍의 가장 큰 장점인 유연성, 재사용성, 유지보수성 등을 모두 잃어버리는 셈이고, OOP를 사용하는 의미가 사라지게 된다.**

### LSP (Liskov Substitution) 리스코프 치환 원칙

- **하위 타입 객체는 상위 타입 객체에서 가능한 행위를 수행**할 수 있어야 함→ 즉, 상위 타입 객체를 **하위 타입 객체로 치환해도 정상적으로 동작**해야 함
- 상속관계에서는 꼭 **일반화 관계 (`IS-A`) 가 성립해야 한다는 의미** (일관성 있는 관계인지)
- **상속관계가 아닌 클래스들을 상속관계로 설정하면, 이 원칙이 위배**됨 (**재사용 목적**으로 사용하는 경우)

### ISP (Interface Segregation) 인터페이스 분리 원칙

- 클라이언트는 **자신이 사용하는 메소드에만 의존**해야 한다는 원칙
- 한 클래스는 **자신이 사용하지 않는 인터페이스는 구현하지 않아야 함**→ 하나의 통상적인 인터페이스보다는 차라리 **여러 개의 세부적인 (구체적인) 인터페이스가 나음**
- 인터페이스는 해당 인터페이스를 사용하는 **클라이언트를 기준으로 잘게 분리되어야 함**

### DIP (Dependency Inversion) 의존 역전 원칙

- 의존 관계를 맺을 때, **변하기 쉬운 것 (구체적인 것) 보다는 변하기 어려운 것 (추상적인 것)에 의존**해야 함
    
    > → 구체화된 클래스에 의존하기 보다는 추상 클래스나 인터페이스에 의존해야 한다는 뜻
    > 
- 즉, **고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 됨**
- 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 함
- **저수준 모듈이 변경되어도 고수준 모듈은 변경이 필요없는 형태가 이상적**

SOLID 원칙을 지키며 설계를 한다면 좋은 객체 지향 설계를 할 수 있다.

그렇다면 SOLID 원칙이 무엇인지 아래와 같이 예시와 함께 정리 해보았다.

### **SOLID**

- SRP: 단일 책임 원칙(single responsibility principle)
- OCP: 개방-폐쇄 원칙 (Open/closed principle)
- LSP: 리스코프 치환 원칙 (Liskov substitution principle)
- ISP: 인터페이스 분리 원칙 (Interface segregation principle)
- DIP: 의존관계 역전 원칙 (Dependency inversion principle)
- **SRP: 단일 책임 원칙(single responsibility principle)**

- 한 클래스는 하나의 책임만 가져야 한다. 이는 클래스를 변경하는데 있어 그 이유가 한가지여야 함을 의미한다.

- SRP 원칙을 잘 지키려면 **책임의 영역을 확실히 구분**하여 서로 **다른 클래스로 분배**해야한다.

- SRP 원칙으로 설계한다면 **변경에서의 연쇄작용이 줄어** 변경에 자유도가 올라간다.

```arduino
class Car {
    private String manufacturerName;// 제조사private String barnd;//브랜드private Long serialNum;//시리얼넘버private String color;//색상private String type;//차 타입(전기, 하이브리드, 일반)private List options;//추가 옵션
}

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

//아래와 같이 책임을 나누어 설계class Car {
    private String serialNum;
    private Manufactuerer manufacturer;
    private CarOption carOption;
}

class Manufactuerer{
	private String manufacturerName;
    private String barnd;
}

class CarOption{
    private color;
    private type;
    private option;
}
```

- **OCP: 개방-폐쇄 원칙 (Open/closed principle)**

- 요소가 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

- 이를 지키려면 **다형성**과 **추상화**를 적극 활용하여야 한다.

- 확장과 변경의 진짜 의미는 구현체를 **직접 변경하는것이 아니라** 추상화된 기능을 여러 구현체로 구현하여 **기능을 확장**하는데 있다.

- 이 또한 변경(아래 예시와 같이 다른 구현체로 변)을 용이하게 하기 위한 원칙

```cpp
// 예시)
MemberRepository m = new MemoryMemberRepository();
MemberRepository m = new JdbcMemberRepository();
```

- **LSP: 리스코프 치환 원칙 (Liskov substitution principle)**

- 프로그램의 객체는 **정확성을 깨뜨리지 않으면서** 하위 타입으로 바꿀 수 있어야 한다

- 이는 컴파일시 부모타입에 자식 타입의 객체가 들어가는 것을 의미하는 것이 아님

- 자식타입은 추상화된 메서드 or **객체의 규약을 지키며 상속받아야함**을 의미한다.

- **ISP: 인터페이스 분리 원칙 (Interface segregation principle)**

- 특정 클라이언트를 위한 하나의 구체적인 인터페이스보다 **여러 범용적인 인터페이스**를 사용해야 한다.

- 이를 통해 인터페이스 명확해지고 대체 가능성이 높아진다.

**아래 예시는 너무 구체적인 잘못된 인터페이스. 전기차는 주유가 불가능하고 옵션을 추가하지 않은 차량은 좌석을 히팅할 수 없다.**

```csharp
// 예시)public interface CarFunc {
    void go(int meter);
    void back(int meter);
    void addOil(int oil);
    void heatingSeat (int heatingGrade);
}
```

- **DIP: 의존관계 역전 원칙 (Dependency inversion principle)**

- 구체화에 의존하지 않고 **추상화에 의존**해야 한다.

- 추상화된 매개체를 통하여 **결합의 관계를 느슨하게 설정**할 수 있다.

```java
// 예시)public class MemberService{
	private MemberRepository m;
}
```