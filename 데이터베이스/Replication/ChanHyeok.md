### Replication

- **Replication이란**

  ![Untitled](https://github.com/5dotseven/cs-basic-study/assets/118906074/30f0d163-351e-4c13-bd36-f4326dfe72c6)

    - 두 개 이상의 DBMS 이용하여 Master/Slave (수직적)구조를 활용하여 DB의 부하를 분산 시키는 기술
    - Master DB에는 Insert, Update, Delete 작업을 수행하도록 하고 Select 작업을 Slave DB에서 하도록 구성
    - 비동기 방식으로 노드들 간의 데이터를 동기화 <br></br>

- **리플리케이션(Replication) 처리 방식**

  ![Untitled](https://github.com/5dotseven/cs-basic-study/assets/118906074/e3a782a5-d0e8-4e0f-8193-714178d259ce)

1. Master 노드에 쓰기 트랜잭션이 수행
2. Master 노드는 데이터를 저장하고 트랜잭션에 대한 로그를 파일에 기록한다.(BIN LOG)
3. Slave 노드의 IO Thread는 Master 노드의 로그 파일(BIN LOG)를 파일(Replay Log)에 복사
4. Slave 노드의 SQL Thread는 파일(Replay Log)를 한 줄씩 읽으며 데이터를 저장

→리플리케이션은 Master와 Slave간의 데이터 무결성 검사(데이터가 일치하는지)를 하지 않는 비동기방식으로 데이터를 동기화한다. 이러한 구조에 의해 리플리케이션 방식은 다음과 같은 장점과 단점을 갖고 있다.
<br></br>
- **장단점**
    - **장점**
        - DB 요청의 60~80% 정도가 읽기 작업이기 때문에 Replication만으로도 충분히 성능을 높일 수 있다.
        - 비동기 방식으로 운영되어 지연 시간이 거의 없다<br></br>
    - **단점**
        - **데이터 정합성을 보장할 수 없음**
            - Slave는 Master의 복사본을 사용하기 때문에 그것이 정말 완벽하고 할 수없습니다. 예를 들어 Slave가 Master의 쿼리 처리량을 따라가지 못한다면 데이터 정합성이 보장되지 않습니다.
        - Master 노드가 다운되면 복구 및 대처가 까다롭다.


출처

- [https://velog.io/@zpswl45/DB-Replication-개념-정리](https://velog.io/@zpswl45/DB-Replication-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC)
- https://mangkyu.tistory.com/97