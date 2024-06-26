### 2. Index

쓰기 작업과 공간을 더 사용하여 DB의 검색속도 향상을 목적으로 한 자료 구조 - 책갈피

인덱스를 사용하지 않는 경우 원하는 대상을 찾기 위해 처음부터 테이블 전체를 조회하는 작업을 진행해야 할 수 있다.

Index 자료구조

- B+tree 인덱스 알고리즘
- Hash 인덱스 알고리즘

인덱스를 사용한다고 모든 상황에서 이점이 있는 것은 아니다.

- 테이블을 변경하는 insert, update, delete 쿼리를 실행하게 되면 이에 따라 추가적으로 index도 변경해야 하므로 추가적인 쿼리가 발생하게 된다.
- 특히 update와 delete 쿼리로 인해 기존의 인덱스가 사용할 수 없게 될 경우 사용불가 처리만 할 뿐 삭제되지 않아 공간을 차지하게 된다.
  → 변경이 자주 일어나는 테이블보다는 읽기만 자주 하는 테이블에 적용