### **2 멀티스레드**

프로세스 안에 여러 개의 스레드를 구성, 각 스레드로 하나의 작업을 처리하게 하는 방식

- 작업마다 프로세스를 생성할 필요가 없으므로 일일히 자원을 할당하는 과정이 감소
- 스레드 간 스택 영역을 제외한 다른 공간은 공유되기 때문에 프로세스 간의 통신보다 빠르고 효율적이다.

<aside>
💡 컨텍스트 스위칭: 프로세스나 스레드가 다른 프로세스나 스레드로 교체되는 과정
cpu 자원을 나누어 사용하고 멀티가 들어간 작업을 가능하게 한다.

</aside>

**단점**

- 자원을 공유하기 때문에 다른 스레드가 사용 중이거나 변형한 데이터에 접근하여 잘못된 데이터 값을 읽어올 수 있다.
- 자원 공유 문제를 해결하기 위해 동기화를 사용하지만 접근 제한으로 인해 오히려 병목 현상을 유발 할 수 있다.
- 데드락, 서로 다른 스레드가 서로 점유하고 있는 자원을 필요로 할 때 서로 가지고 있는 자원을 놓아주지 못해 무한 대기 상태에 빠지게 된다.
  → 멀티 프로세스에서도 발생하는 문제
- 멀티프로세스와 다르게 하나의 스레드에 문제가 발생하면 전체 스레드에 영향을 준다.
  → 프로그래머의 적절한 예외처리, 에러 발생 시 다른 스레드 사용 등 역량에 따라 해결이 가능