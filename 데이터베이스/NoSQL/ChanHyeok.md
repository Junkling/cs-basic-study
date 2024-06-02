### NoSQL

- NoSQL란?
    - **Not Only SQL의 약자**
    - **기존 RDB의 한계를 극복하고, 수평적인 확장성을 가짐**
    - **유연한 스키마를 통해 유연성과 확장성을 가짐**
    - **ex) MongoDB**
    - **장점**
        - **수평적인 확장이 쉽다**
        - **데이터의 저장 및 검색을 위한 특화된 매커니즘 제공-> 응답속도나 처리효율 등에 있어서 매우 뛰어난 성능**
        
- CAP 이론
    - 분산형 컴퓨팅은 Consistency, Availability, Partition tolerance 3가지 특징 지님,
    CAP이론은 이중 2가지만 만족할 수 있다는 이론
    RDBMS는 ACID 특성 따르지만, 분산형 구조인 NoSQL은 CAP 이론을 따른다.
    - Consistency(일관성) : 분산된 노드 중 어느 노드로 접근하더라도 같은 데이터 값
    - Availability(가용성)
        1. 한 노드가 다운되어도 다른 노드에 문제가 생기지 않아야 함
        2. 다중 클라이언트에서 모든 클라이언트의 읽기와 쓰기 요청에 대하여 항상 응답이 가능해야 함을 보증하는 것
    - Partition tolerance(분할 허용성)
        1. 일부 메시지를 손실해도 시스템이 정상 동작해야 함
        2. 지역적으로 분할된 네트워크 환경에서 동작하는 시스템에서 두 지역 간의 네트워크가 단절되거나 네트워크 데이터의 유실이 일어나더라도 각 지역 내의 시스템은 정상적으로 동작해야 함을 의미
        
- 저장방식에 따른 분류
    - Key-Value Model
        - 가장 기본적이고 단순한 형태로, 대부분의 NoSQL에서 지원한다.
        - 키 하나로 데이터 하나를 저장하고 조회할 수 있는 단일 키-값 구조이다.
            - 유니크 포인터를 사용
            - Key에는 어떠한 값도 사용 가능
        - EX) Redis, DynamoDB, Oracle NoSQL Database, Apache Ignite
        - 특징
            - 복잡한 조회 연산을 지원하지 않음
            - 고속 읽기와 쓰기에 최적화된 경우가 많음 (고성능)
            - 하나의 서비스 요청에 다수의 데이터 조회 및 수정 연산이 발생하면 트랜잭션 처리가 불가능하여 데이터 정합성을 보장할 수 없음
    - Document Model
        - NoSQL의 경우 데이터 관계를 정의하지 않기 때문에 이런 문제가 발생할 이유가 없고, 때문에 document 형태로 데이터를 관리
        - 모든 데이터를 하나의 테이블에 보관
        - 키-값 모델을 개념적으로 확장한 구조
        - Document = 포장되고(encapsulated) 인코딩된 데이터
            - XML, YAML, JSON을 주로 사용
        - 하나의 키에 하나의 구조화된 문서를 저장하고 조회
            - key: document ID, value: 구조화된 데이터 타입(XML, JSON, YAML, ...)
        - Ex)MongoDB, IBM Domino, CouchDB
    - Column Model
        - RDBMS에서는 데이터를 row 단위로 저장한다면, column model은 column 단위로 저장한다. 각 column은 column family로 묶이고, 이 family들은 또 다른 column이 될 수 있다.
        - 하나의 키에 여러 개의 컬럼 이름과 컬럼 값의 쌍으로 이루어진 데이터를 저장하고 조회
        - 모든 컬럼은 항상 타임 스탬프 값과 함께 저장
            - Column: 데이터 저장의 기본 단위 (column name + column value + time stamp)
        - 특징
            - 데이터 수정 시 전체 row가 아니라 특정 row만 로드하면 되므로 불필요한 I/O 작업이 줄어든다.
            - 데이터는 하나의 연속적인 엔티티로 취급되며, 조회 시 다른 row로 이동하는 등의 불필요한 작업이 필요 없다.
                - 조회가 굉장히 빠르다.
            - 하나의 row에 삽입되는 데이터의 형태가 동일하므로 DB의 블록 하나에는 한 유형의 데이터만 존재하고, 압축이 가능하여 디스크 공간을 절약할 수 있다.
        - Ex) BigTable, AWS Redshift, Accumulo, Cassandra
    - Graph model
        - 데이터 뿐만 아니라 데이터 간 관계도 데이터로 취급한다.
        - 특징
            - 데이터들의 관계를 중요시해서 저장된 데이터들이 엣지로 직접 연결될 수 있다.
                - 데이터 질의 시 특정 노드와 관련된 데이터를 한 번의 연산으로 획득이 가능하다.
            - 고도로 연결된 데이터 셋을 사용하는 애플리케이션을 쉽게 구축하고 실행하는 데에 유리하다.
        - Ex) Neo4j, Giraph, IBM DB2
    
    ## ***예상 면접 질문 및 답변***
    
    ### ***NoSQL은 무엇인가?***
    
    NoSQL은 관계형 데이터베이스의 한계를 보완하기 위해 고안된 데이터베이스 모델로, 느슨한 스키마를 제공하여 분산 컴퓨팅에서의 유연성, 확장성, 고성능성, 고기능성을 보장한다.
    
    ### ***CAP 이론에 대해 설명하라***
    
    CAP는 Consistency, Availability, Partition tolerance, 3가지 특성의 약자이며, CAP 이론은 분산 컴퓨팅 환경에서 데이터 모델은 3가지 특성을 전부 가질 수는 없다는 이론이다.
    
    이 이론에 따라 데이터베이스들은 2가지 특성을 가지며, 데이터를 어떠한 관점으로 볼 것인가에 따라 어떤 특성을 가져갈 것인지 선택하게 된다.
    
    - consistency (일관성) → 클라이언트들이 같은 시간에 조화하는 데이터는 항상 동일해야 한다.
    - availability (가용성) → 모든 클라이언트들의 읽기/쓰기 요청에 대해 항상 응답이 가능해야 한다.
    - partition tolerance (분할 내성) → 분할 네트워크 환경에서 네트워크가 끊기거나 데이터가 유실되어도 각 환경 내에서는 정상적으로 동작되어야 한다.
    
    관계형 데이터베이스의 경우 consistency + availability 특성을 갖지만, NoSQL의 경우 consistency + partition tolerance 특성 혹은 availability + partition tolerance 특성을 갖는다.
    
    ### ***NoSQL 데이터베이스 모델을 분류하라.***
    
    NoSQL은 다양한 모델을 기반으로 데이터베이스를 설계할 수 있는데, 크게 4가지로 분류할 수 있다.
    
    - Key-Value
    - Document
    - Column
    - Graph
    
    출처
    
    - https://steady-coding.tistory.com/551
    - https://hyewon-study-log.tistory.com/132