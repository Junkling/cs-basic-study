### 5. Statement vs PreparedStatement

캐시 사용의 유무가 가장 큰 차이점

- Statement - SQL문을 실행할 때마다 매 번 구문을 새로 작성하고 해석, 오버헤드가 발생
- PreparedStatement - 선처리된 SQL 구문, SQL문을 미리 준비해 놓고 바이딩 변수를 사용해서 반복되는 비슷한 SQL 문을 처리

**동작 방식**

1. 구문 분석, 정규화: 문법 확인 및 DB와 테이블 존재 여부 확인
2. 컴파일
3. Query 최적화
4. 캐시
5. 실행

PreparedStatement는 최초 실행 시에만 1부터 5까지 단계를 거치고 이후부터는 바로 4번 단계부터 실행이 가능하다.

```java
// 출처: https://shuu.tistory.com/129
[Statement]
Connection conn = DriverManager.getConnection(url, id, pwd);
Statement stmt = conn.createStatement();
stmt.executeUpdate("Update Member Set Name = " + "머스크햄" Where id = " + 10);
stmt.executeUpdate("Update Member Set Name = " + "화성갈끄니까" Where id = " + 11;

[PreparedStatement]
Connection conn = DriverManager.getConnection(url, id, pwd);
String sql = "Update Member Set Name = ? Where id = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, "머스크햄");
pstmt.setInt(2, 10);
pstmt.executeUpdate();

pstmt.setString(1, "화성 갈끄니까");
pstmt.setInt(2, 11);
pstmt.executeUpdate();
출처: https://shuu.tistory.com/129 [All about IT:티스토리]
```