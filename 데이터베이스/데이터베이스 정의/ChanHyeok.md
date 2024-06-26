- 데이터베이스
    - **데이터베이스의 특징**
    1. 데이터의 독립성
        - 물리적 독립성 : 데이터베이스 사이즈를 늘리거나 성능 향상을 위해 데이터 파일을 늘리거나 새롭게 추가하더라도 관련된 응용 프로그램을 수정할 필요가 없다.
        - 논리적 독립성 : 데이터베이스는 논리적인 구조로 다양한 응용 프로그램의 논리적 요구를 만족시켜줄 수 있다.
    2. 데이터의 무결성
    여러 경로를 통해 잘못된 데이터가 발생하는 경우의 수를 방지하는 기능으로 데이터의 유효성 검사를 통해 데이터의 무결성을 구현하게 된다.
    3. 데이터의 보안성
    인가된 사용자들만 데이터베이스나 데이터베이스 내의 자원에 접근할 수 있도록 계정 관리 또는 접근 권한을 설정함으로써 모든 데이터에 보안을 구현할 수 있다.
    4. 데이터의 일관성
    연관된 정보를 논리적인 구조로 관리함으로써 어떤 하나의 데이터만 변경했을 경우 발생할 수 있는 데이터의 불일치성을 배제할 수 있다. 또한 작업 중 일부 데이터만 변경되어 나머지 데이터와 일치하지 않는 경우의 수를 배제할 수 있다.
    5. 데이터 중복 최소화
    데이터베이스는 데이터를 통합해서 관리함으로써 파일 시스템의 단점 중 하나인 자료의 중복과 데이터의 중복성 문제를 해결할 수 있다.
    
- SQL(**Structured Query Language)의**
    - **관계형 데이터베이스 시스템(RDBMS)의 데이터 베이스가 이해할 수 있는 언어**
    - **종류**
        - **DDL(Data Definition Language)**
            - **테이블이나 관계의 구조를 생성하는데 사용되는 데이터 정의어**
            - **CREATE / ALERT / DROP 등**
        - **DML(Data Manipulation Language)**
            - **테이블의 데이터를 검색, 삽입, 수정, 삭제하는데 사용되는 데이터 조작어**
            - **SELECT / INSERT / DELETE / UPDATE**
        - **DCL(Data Control Language)**
            - **데이터의 사용 권한을 관리하는데 사용되는 데이터 제어어**
            - **GRANT / REVOKE**
- DBMS
    - 사용자와 데이터베이스 사이에서 사용자의 요구에 따라 정보를 생성하고, DB를 관리해주는 소프트웨어
    - 사용자의 질의를 처리해주는 다양한 컴파일러가 있고, 데이터베이스 관리자가 DBMS를 관리한다
- **RDB**
    - RDB(Relational Database)
        - 데이터를 2차원 형태의 테이블로 표현하는 데이터 모델에 기초를 둔 DataBase
        - 엄격하게 정해진 스키마에 따라 데이터를 저장하기에 명확한 데이터 구조를 보장하는 DB
            - 스키마 : DB를 구성하는 데이터 개체, 속성, 관계등 다양한 제약조건을 정의하는 것
        - 중복 데이터가 존재하지 않아서 데이터 수정(update)이 용이하며 테이블이 많아질 경우 JOIN으로 인해 많은 쿼리가 발생하는 단점이 있다
        - 수평적인 확장이 어렵다
        - RDBMS
            - 관계형 데이터베이스를 생성하고 수정하고 관리할 수 있는 소프트웨어
            - ex) MySQL / Oracle / Maria-DB 등