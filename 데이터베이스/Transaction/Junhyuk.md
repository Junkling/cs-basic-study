# 트랜잭션

트랜잭션은 데이터베이스의 상태를 변환시키는 기능을 수행할 때 작업 단위별로 한번에 모두 수행되어야 할 연산을 의미한다.

### 트랜잭션 특징

- 데이터베이스 시스템에서 병행 제어 및 회복 작업시 처리되는 작업의 논리적 단위
- 하나의 트랜잭션은 Commit되거나 RollBack되야한다.

### 트랜잭션 성질

**Atomicity(원자성)**

- 트랜잭션의 연산은 데이터베이스에 모두 반영되는 것이 아니면 모두 반영되지 않아야한다.
- 트랜잭션 내 모든 명령을 반드시 완벽히 수행해야한다. 하지 못하면 전부 취소되야한다.

**Consistency(일관성)**

- 트랜잭션이 성공적으로 실행 완료하면 언제나 일관성 있는 데이터 베이스 상태가 되어야한다 (예를 들어 데이터를 조회하면 모두 동일한 데이터를 반환해야함)
- 시스템이 가지고 있는 고정 요소는 트랜잭션 수행 전과 수행 후 상태가 같아야한다.

**Isolation(독립성,격리성)**

- 둘 이상의 트랜잭션이 동시에 실행되는 경우 어느 하나의 트랜잭션 실행중에는 다른 트랜잭션이 끼어들 수 없다.
- 수행중인 트랜잭션이 완전히 종료될때 까지 다른 트랜잭션은 수행 결과를 참조할 수 없다.

**Durablility(영속성,지속성)**

- 성공적으로 완료된 트랜잭션의 결과는 영구적으로 반영되어야 한다.

# 트랜잭션 격리 수준

트랜잭션 격리수준 4가지

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE

1. read uncommitted == dirty read
- 거의 사용되지 않음
- 트랜잭션의 변경 내용이 commit이나 rollback 여부에 상관 없이 보임
    - 에러가 발생해서 rollback된 항목을 트랜잭션 중간에 접근할 경우 rollback되기 전 데이터가 보임
1. read committed
    - 트랜젝션이 완료된  데이터만 다른 트랜잭션에서 조회 가능
    - 커밋되기 전에는 언두로그에 있는 곳의 데이터를 읽어옴
        - 언두로그 → 뭔가 잘못되면 돌려야돼서 임시로 저장하는 공간
    - unrepeatable read
        - 커밋 전에 불러오면 변경되기 전 데이터를 가져오고,
        - 커밋 후에 불러오면 변경된 데이터로 가져온다.
        - 문제 없어 보이지만 같은 트랜잭션 안에서너 같은 값을 가져와야한다는 정합성에 어긋남
2. repeatable read
    - 언두 영역에 백업된 이전 데이터를 이용해서(언두로그의 번호값으로) 다른 트랜잭션에서 데이터를 변경하더라도 동일 트랜잭션에서는 같은 내용을 보여줄 수 있도록 함
    - phantom read
        - 언두 레코드에는 lock을 걸 수 없어서 같은 트랜잭션에서 조회 가능
        - 다른 트랜잭션에서 새로운 데이터를 INSERT하고 Commit했을 시 언두로그의 데이터까지 조회해 옴으로 같은 트랜잭션 안에서 조회값이 달라진다.
        - 왔다 갔다 해서 phantom read(유령)이라고 함
3. serializable 
    - 동시성이 중요한 데이터베이스의 경우 거의 사용되지 않음
    - read도 lock을 획득해야만 가능함
        - read가 lock을 가지고 있기 때문에, write나 update, delete등을 실행할 수 없음
