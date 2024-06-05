### 샤딩

- **샤딩이란?**
    - 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법
    - 보통 전체 데이터베이스 하나의 테이블에 전부 들어가기 힘든 데이터가 등장하고 DBMS가 테이블을 관리하기 힘들어짐에 따라 적용
    - 수평 파티셔닝과 유사<br></br>
    - **장단점**
        - **장점**
            - Scale-Out이 가능
            - 스캔 범위를 줄여서 쿼리 반응 속도를 빠르게 함
            - 장애가 샤드 단위로 발생함<br></br>
        - **단점**
            - 프로그래밍 복잡도가 증가
            - 데이터가 한 쪽 샤드로 몰릴 경우(Hotspot), 샤딩이 무의미 해짐
            - 잘 못 사용할 경우 risk가 큼
            - 한번 샤딩 사용시 샤딩 이전의 구조로 돌아가기 힘들다.<br></br>
    - **종류**
        - **모듈러 샤딩(Modular Sharding)**

          ![Untitled (2)](https://github.com/5dotseven/cs-basic-study/assets/118906074/02ff1bf1-ef0b-4c9b-9139-a1cfe3f19256)

            - 정의: PK를 모듈러 연산한 결과로 DB를 라우팅하는 방식
            - 특징
                - 레인지 샤딩에 비해 데이터가 균일하게 분산된다.
                  → 데이터가 균일하게 분산된다는 점은 트래픽을 안정적으로 소화하면서도 DB 리소스를 최대한 활용할 수 있다는 것을 의미한다.
                - DB를 추가 증설하는 과정에서 이미지 적재된 데이터의 재정렬이 필요하다.
                - 데이터가 일정 수준에서 예상되는 데이터 성격을 가진곳에 적용할 때 어울린다

              → 데이터가 늘어남에 따라 샤딩을 **추가적으로 해야하는 상황**이 자주 생기면 **큰 부하가 발생**하기 때문에 활용<br></br>

        - **레인지 샤딩(Range Sharding)**

          ![Untitled (3)](https://github.com/5dotseven/cs-basic-study/assets/118906074/f25bb02a-00eb-4533-a61c-f0ec01ccc590)

            - 정의: **PK의 범위를 기준**으로 DB를 특정하는 방식입니다.
            - 특징
                - 모듈러 샤딩에 비해 기본적으로 증설에 재정렬 비용이 들지 않는다.
                - 일부 DB에 데이터가 몰릴 수 있다

              → 레인지 샤딩의 가장 큰 장점은 증설 작업에 드는 큰 비용이 들지 않는다는 점이다. 데이터가 급격히 증가할 가능성이 있을 때 사용<br></br>

        - **디렉토리 샤딩(Directory Sharding)**

          ![Untitled (3)](https://github.com/5dotseven/cs-basic-study/assets/118906074/f25bb02a-00eb-4533-a61c-f0ec01ccc590)

            - 정의 : 별도의 조회 테이블을 사용해서 샤딩을 하는 경우
            - 특징
                - 샤딩에 사용되는 시스템이나 알고리즘을 사용할 수 있다.
                - 샤드를 동적으로 추가하는 것도 비교적 쉽다.
                - 모든 읽기 및 쓰기 쿼리 전에 조회 테이블을 참조해야 하므로 오버헤드가 발생한다.<br></br>

출처

- [https://medium.com/@taesulee93/database-sharding과-partitioning-b2098b788b38](https://medium.com/@taesulee93/database-sharding%EA%B3%BC-partitioning-b2098b788b38)
- https://repeater2487.tistory.com/41
- [https://velog.io/@kyeun95/데이터베이스-샤딩Sharding이란](https://velog.io/@kyeun95/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%83%A4%EB%94%A9Sharding%EC%9D%B4%EB%9E%80)