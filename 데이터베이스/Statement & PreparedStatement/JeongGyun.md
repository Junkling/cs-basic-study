# 데이터베이스 - Statement vs PreparedStatement
- 쿼리 실행 과정
    - **Statement**: 쿼리를 실행할 때마다 매번 구문 분석, 컴파일, 실행 단계를 거쳐야 합니다.
    - **PreparedStatement**: 처음 한 번만 구문 분석, 컴파일 단계를 거치고 이후에는 실행 단계만 거치면 됨
- 쿼리 문자열 처리
    - **Statement**: 쿼리 문자열을 직접 작성해야 함
    - **PreparedStatement**: 쿼리 문자열에 변수 부분을 물음표(?)로 표시하고, 별도의 메서드로 변수 값을 설정함
- 보안
    - **Statement**: 쿼리 문자열에 사용자 입력이 포함되어 SQL 인젝션 공격에 취약함
    - **PreparedStatement**: 변수 부분을 별도로 설정하므로 SQL 인젝션 공격을 방지할 수 있음
- 성능
    - **Statement**: 매번 구문 분석, 컴파일 단계를 거쳐야 하므로 상대적으로 느립니다.
    - **PreparedStatement**: 처음 한 번만 구문 분석, 컴파일 단계를 거치므로 빠름

### 정리
Statement는 DDL(CREATE, ALTER, DROP) 구문을 처리할 때 적합
매 실행시 Query를 다시 파싱하기 때문에 속도가 느리며, SQL Injection공격에 취약함

Prepared Statement는 DML(SELECT, INSERT, UPDATE, DELETE)구문 처리에 적합
그리고 캐시에 저장된 Query를 활용하기 때문에 실행이 빠르며 SQL Injection을 막기 위한 방법으로 활용됨