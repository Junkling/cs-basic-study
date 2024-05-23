# 프로세스 동기화

### 프로세스 동기화

- **정의** : 다중 프로세스 환경에서 자원에 데이터 일관성을 유지하도록 하는 것
- **필요성** : 데이터에 동시 접근시 데이터 일관성이 깨지거나 결과가 잘못될 가능성이 있다.

### 데이터 일관성 문제 (Data Consistency Problem)

- 두 개 이상의 프로세스나 쓰레드가 데이터를 공유하는 환경에서 서로 교차적으로 데이터를 사용(use, access, reference, read-write, manipulate)할 때 발생하는 문제

### Race Condition

- 공유 자원에 여러 프로세스나 스레드가 접근할 경우 **접근 순서**에 따라 결과가 달라지는 현상

**뮤텍스 락(Mutex Lock) vs 세마포어(Semaphore)**

### 뮤텍스 락(Mutex Lock)

![image](https://velog.velcdn.com/images%2Fwoosung0420k%2Fpost%2F380eb049-bb26-472e-9c9a-c334194c3911%2Fimage.png)

- 이진 세마포어의 일종으로 자원에 lock을 걸면서 동기화 문제를 해결한다.
- 상호 배제 개념을 이용하며 Critical Section을 가진 쓰레드들이 각각 단독으로 실행되게 하는 기술
- acquire() : Lock을 획득
- release() : Lock을 반환

### 세마포어 (Semaphore)

![image](https://velog.velcdn.com/images%2Fwoosung0420k%2Fpost%2F4f5e57a4-b249-4b9a-a0e3-80c183836cd8%2Fimage.png)

- 자원을 할당하는 P연산과 자원을 해제하는 V연산이 있다.
- Critical Section에 들어가기 전 세마포어의 Counter 변수를 통해 자원에 접근가능한지 확인한다.
- wait() : Critical Section에 들어가면서 세마포어 변수 1 감소 (acquire과 유사)
- signal() : Critical Section에 들어가면서 세마포어 변수 1 증가 (release와 유사)
