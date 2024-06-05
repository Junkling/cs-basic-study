# 파티셔닝

서비스의 크기가 점점 커지고 DB에 저장하는 데이터의 규모 또한 대용량화 됨에 따라 DB 시스템의 용량(storage)의 한계에 근접해지고 성능(performance)의 저하가 생기게 된다.

파티셔닝은 이러한 시점에 데이터베이스를 튜닝 기법 중 하나이다. 이를 통해 데이터베이스를 분산 처리하여 성능이 저하되는 것을 방지하고 보다 수월하게 관리  할 수 있다.

즉, 큰 테이블이나 인덱스를 작은 파티션(Partition) 단위로 나누어 관리하는 기법을 뜻한다.

### 장점

**성능**

- 특정 쿼리의 성능을 향상시킨다.
- 대용량 데이터의 쓰기에 효율적임
- 필요한 데이터만 빠르게 조회할 수 있다
- Full Scan의 범위가 줄어든다.

**가용성**

- 물리적인 파티셔닝으로 전체 데이터의 훼손을 막을 수 있다.
- 파티션을 독립적으로 백업하고 복구할 수 있다.
- Update 성능을 향상시킬 수 있다.

**관리**

- 큰 테이블들을 제거하여 관리가 쉬워진다.

### 단점

- 파티셔닝은 테이블을 여러 파티션으로 쪼개기 때문에 Join비용이 증가한다.

### 파티셔닝 방법

**수직 파티셔닝**

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbe75d3b8-e9b1-4a63-86c9-4ea1a2c2dbb8%2FUntitled.png?table=block&id=8f28517f-4ff4-4f77-aa72-b959502496fb&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

하나의 테이블의 각 행을 다른 테이블에 분산시키는 것이다.

샤딩(Sharding) 과 동일한 개념으로 복제된 스키마(schema)에 데이터를 나누어 적재하는 방식.
즉, 같은 스키마 데이터를 여러 테이블에 나누어 저장하는 것을 말한다.

**장단점**

**장점**

- 데이터의 개수가 작아지고 따라서 index의 개수도 작아지게 된다. 자연스럽게 성능은 향상된다.

**단점**

- 서버간의 연결과정이 많아진다.
- 데이터를 찾는 과정이 기존보다 복잡해진다.
    - 레이턴시 상승
- 특정 서버(테이블)에 문제가 생기면 데이터의 무결성이 깨질 수 있다.

**수평 파티셔닝**

테이블의 일부 열을 빼내는 형태로 분할한다.

모든 컬럼들 중 특정 컬럼들을 쪼개서 따로 저장하는 형태를 의미한다.

하나의 엔티티를 2개 이상으로 분리하는 하여 스키마(schema)를 나누고 데이터를 나누는 것을 의미

이는 제3 정규화 방식과 유사하다 but 수직 파티셔닝은 정규화 된 데이터를 분할하는 과정

**장단점**

**장점**

- 자주 사용하는 컬럼 등을 분리 시켜 성능을 향상시킬 수도 있다.
- 필요한 컬럼만 SELECT 하여 메모리에 올리는 등 성능상의 이점이 있다.
- 같은 타입의 데이터가 저장되기 때문에 저장 시 데이터 압축률을 높일 수 있다.

**단점**

- 저장 및 조회에 대한 로직이 복잡해질 수 있다.
- 조회시 Join이 많아지는 문제가 생길 수 있다.
- 데이터를 찾는 과정이 기존보다 복잡해진다.
    - 레이턴시 상승

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcab195fb-fc46-43bd-a133-28a62e81b3fb%2FUntitled.png?table=block&id=1f9f4d21-5193-46a8-ad3f-8667c07ab10a&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

### Partition 방식

![image](https://blog.kakaocdn.net/dn/bxEjuM/btrXz0CVnvu/AYdxlSGQ3u312MOAk1bIK1/img.gif)

![image](https://blog.kakaocdn.net/dn/th56I/btrXCIOwTRq/FPHOvPZbLiRR5jKATBGrd0/img.gif)

- Range partitioning - 연속적인 숫자나 날짜 기준으로 파티셔닝한다.
- List partitioning - 특정 내용에 리스트 형식의 키 값 기준으로 파티셔닝한다.
- Hash Partitioning - 범위가 없는 데이터를 Hash 기준으로 균등하기 파티셔닝한다.
- Composite partitioning - 파티셔닝을 진행 했을 때 파티셔닝 자체가 너무 클 때 유용하게 사용된다. Range-hash는 Range 파티셔닝을 진행 하였지만 이마저도 커서 Hash 파티셔닝을 다시 한 모양이다.