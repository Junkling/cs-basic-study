# 샤딩

 **대규모 분산 데이터베이스 시스템에서 데이터를 수평적 확장을 통해 여러 서버에 분산 저장하는 기술** 샤딩은 수평 파티셔닝의 큰 범주내에 있으며 수평적 파티셔닝은 동일한 DB서버 내에서 테이블을 분할하는 것이고 샤딩은 DB서버를 분할하여 DB서버의 부하를 분산 할 수 있다.

### 종류

**1 모듈러 샤딩**

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F59a97234-999e-47d4-892f-d22e0d177e2d%2FUntitled.png?table=block&id=349116c9-2f90-4e42-b083-53371efc6c61&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

- 균일하게 데이터를 분산시킨다.
- DB를 추가 증설하는 과정에서 데이터 재정렬이 필요하다.
- 균일하게 분산되어 트래픽을 안정적으로 소화할수 있으나 데이터가 늘어나 샤딩을 늘려야 하는 경우 단점이 있다.

**2 레인지 샤딩**

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F66c738c0-492a-4b4d-b35e-db7c35aa436a%2FUntitled.png?table=block&id=37978891-4618-4001-b24d-b20d85ef62d1&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

- 증설에 대한 재정렬 비용이 없다.
- 일부 DB에 데이터가 몰려 분산 효과가 크지 않을 수 있다.

**3 디렉토리 샤딩**

별도 조회 테이블로 샤딩을 관리하는경우

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa05cfbac-83e7-49c2-b56f-ac675f73d0b2%2FUntitled.png?table=block&id=e7e39447-87b4-45f9-beb3-252f09bf9470&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

- 샤딩에 사용되는 시스템이나 알고리즘을 사용할 수 있다.
- 샤드를 동적으로 추가하는 것도 비교적 쉽다.
- 모든 읽기 및 쓰기 쿼리 전에 조회 테이블을 참조해야 하므로 오버헤드가 발생.