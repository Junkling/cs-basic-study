# Replication
서비스의 사용자가 많아지면 커넥션 풀의 부하로 인해 데드락 상태가 발생할 가능성이 높아진다.
이러한 부하를 덜기 위해 DB를 Master와 Slave 로 나누어 커넥션의 부하를 줄이고 두 DB의 기능을 나누는 등 성능을 향상하기 위해 복재함

• 두 개의 이상의 DBMS 시스템을 Master / Slave 로 나눠서 동일한 데이터를 저장하는 방식이다.

Master DBMS에는 데이터의 수정사항을 반영만하고 Replication 하여 Slave DBMS들에 실제 데이터를 복사한다.

Master DBMS는 INSERT, UPDATE, DELETE 만 수행하고

Slave DBMS는 SELECT 만 하도록 구성하는것

장점: 

- 데이터 조회에 대한 성능이 상승 

- 데이터 백업용도로 사용도 가능하다.

단점: 

- 데이터 정합성을 보장할 수는 없다.

- 로그가 Master가 Slave의 로그를 관리하지 못하기 때문에 Master의 로그 보관주기와 Slave의 로그 보관 주기가 일치할 수 없다.

Master 노드가 다운되면 복구가 어렵다.

### **로그기반 복제(Binary Log)**

- Statement Based : `SQL문장`을 복사하여 진행
    - issue : SQL에 따라 결과가 달라지는 경우(Timestamp, UUID, …)
- Row Based : SQL에 따라 변경된 `Row 라인`만 기록하는 방식
    - issue : 데이터가 많이 변경된 경우 데이터 커질 수 밖에 없다.
- Mixed : 기본적으로 Statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.

## Replication 순서도
![image](https://velog.velcdn.com/images%2Fzpswl45%2Fpost%2F53d75593-4db1-442f-9033-9eeed22c1fc2%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.53.14.png)