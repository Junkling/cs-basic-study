**Statement** 

- Statement 종류에는 Statement, PreparedStatement, CallableStatement
    - CallableStatement는 PL/SQL문을 호출할 때 사용한하지만 성능 이슈로 사용X
- **Statement**
    - 특징
        1. 단일로 사용될 때 빠른 속도를 지닙니다.
        2. 쿼리에 인자를 부여할 수 없습니다. →변수 설정+바인딩에서 static sql 사용
        3. 매번 컴파일을 수행해야 합니다.
    - 동작 방식
        - Parsing(문장 분석) : 먼저 애플리케이션은 문의 틀을 만들고 DBMS로 보낸다. 특정값은 지정하지 않은 채로 남겨진다.
        - Compile(컴파일) : DBMS는 문의 틀을 컴파일하며(최적화 및 변환) 아직 실행하지 않고 결과만 저장한다.
        - Execute(실행) : 나중에 애플리케이션이 SQL문 틀에 있는 변수에 값(바인드)을 지정하면 DBMS는 (결과를 반환할 수도 있는) SQL문을 실행한다. (애플리케이션은 여러 값으로 원하는 회수만큼 SQL문을 실행할 수 있다.)

- **PreparedStatement**
    - DBMS에서 동일하거나 비슷한 데이터베이스의 SQL문을 높을 효율성으로 반복적으로 실행하기 위해 사용되는 기능.
    - 특징
        1. 쿼리에 인자를 부여할 수 있습니다.
        2. 처음 프리컴파일 된 후, 이후에는 컴파일을 수행하지 않습니다.
        3. 여러번 수행될 때 빠른 속도를 지닙니다.
        4. 쿼리 자체에 조건이 들어가는 dynamic sql 사용
        5. SQL Injection 문제 보완
    - 동작 방식
        - Parsing(문장 분석) : 먼저 애플리케이션은 문의 틀을 만들고 DBMS로 보낸다. 특정값은 지정하지 않은 채로 남겨진다.
        - Compile(컴파일) : DBMS는 문의 틀을 컴파일하며(최적화 및 변환) 아직 실행하지 않고 결과만 저장한다.
        - Execute(실행) : 나중에 애플리케이션이 SQL문 틀에 있는 변수에 값(바인드)을 지정하면 DBMS는 (결과를 반환할 수도 있는) SQL문을 실행한다. (애플리케이션은 여러 값으로 원하는 회수만큼 SQL문을 실행할 수 있다.)
    - Statement vs PreparedStatement
        - 캐시의 사용여부
            - **SQL의 실행 단계**
                1. 쿼리문 분석
                2. 컴파일
                3. 실행
            - Statement : 매번 쿼리 수행마다 3단계를 수행.(계속적으로 단계 수행)
            - PreparedStatement : 처음 1번만 3단계를 거친 후 캐시에 담아 재사용 가능.
            -> 동일한 쿼리를 반복적으로 수행하면 PreparedStatement가 DB에 적은 부하를 주고, 성능도 좋다.
        - PreparedStatement를 사용해야 하는 경우
            - 사용자 입력값으로 쿼리를 생성하는 경우
            - 쿼리 반복수행 작업일 경우
        - Statement 반드시 사용해야 하는 경우
            - Dynamic SQL을 사용할 경우 (매번 조건절이 틀려지므로 PreparedStatement의 캐싱 장점을 잃어버림)
        

출처:

[https://shuu.tistory.com/129#(3) Statement vs PreparedStatement 차이점-1](https://shuu.tistory.com/129#(3)%20Statement%20vs%20PreparedStatement%20%EC%B0%A8%EC%9D%B4%EC%A0%90-1)

https://p2-study.tistory.com/98

https://excited-hyun.tistory.com/111?category=946660

https://jungkeung.tistory.com/68