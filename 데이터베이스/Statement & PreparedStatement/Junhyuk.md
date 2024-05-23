SQL문을 실행할 수 있는 객체

차이점은 캐시 사용 여부

### **SQL 실행 단계**

1. 쿼리 문장 분석
2. 컴파일
3. 실행

```java
String sqlstr = "SELECT name, memo FROM TABLE WHERE num = " + num 

Statement stmt = conn.credateStatement(); 

ResultSet rst = stmt.executeQuerey(sqlstr);
```

실행 시점에 sqlstr을 완성시킴

Statement 는 executeQuery() 나 executeUpdate() 를 실행하는 시점에 파라미터로 SQL문을 전달하는데, 이 때 SQL문을 수행하는 과정에서 매번 컴파일을 하기 때문에 성능상 이슈가 있다.

- 쿼리문을 수행할 때마다 SQL 실행 단계 1) ~ 3)의 과정을 거침
- SQL문을 수행하는 과정에서 매번 컴파일을 하게 때문에 성능상 이슈 발생
- 실행되는 SQL 문을 확인 가능

```java
String sqlstr = "SELECT name, memo FROM TABLE WHERE num = ? " 

PreparedStatement stmt = conn.prepareStatement(sqlstr); 

pstmt.setInt(1, num);

ResultSet rst = pstmt.executeQuerey();
```

sqlstr 은 생성시에 실행

PreparedStatement은 준비된 Statement 이다. 

이 준비는 컴파일(Parsing) 을 이야기하며, 컴파일이 미리 되어 있어 Statement 에 비해 성능상 이점이 있다.

- 컴파일이 미리 되어있기 때문에 Statement에 비해 좋은 성능
- 특수 문자를 자동으로 파싱해주기 때문에 SQL Injection 같은 공격을 막을 수 있음
- '?' 부분에만 변화를 주어 쿼리문을 수행하므로 실행되는 SQL문을 파악하기 어려움

파라미터 바인딩이 왜 안전한가

## 파라미터 바인딩을 사용하면 안전한 이유

**3줄 요약** : PreparedStatement의 내부에는 파라미터를 인코딩하여 보내는 기능이 포함되어 실제 파라미터를 외부에서 확인하기가 어렵기 때문

### org/h2/jdbc/JdbcPreparedStatement.java

![image](https://blog.kakaocdn.net/dn/brhhmp/btrMjdfV8tL/ivagWd0ev1ukkagQwvlRaK/img.png)

### PreparedStatement 내부

quote내부로 들어가면 아래와 같이 **StringUtils.quoteJavaString()** 메서드가 나온다.

![image](https://blog.kakaocdn.net/dn/A65Ni/btrMjWxIfgu/2cgPBwHXZzF542KEUQ1Qe1/img.png)

### quote

해당 메서드에 들어가면 아래가 나오고 다시 javaEncode에 들어간다.

![image](https://blog.kakaocdn.net/dn/ckS2vT/btrMnKP5dZL/ZSmMmettKJMvHzDXB9hKLk/img.png)

### quote 내부

아래처럼 String으로 들어온 값들을 하나씩 돌면서 java스타일에 맞게 인코딩해준다.

![image](https://blog.kakaocdn.net/dn/2bUbO/btrMlUMfy8E/spjn41fOML7p2FknbHltNk/img.png)

**자동 인코딩**

위와 같이 자동으로 sql injection에 대해서 막아준다. 즉, Prepared Statement를 사용하면 인자를 넣어주기 전의 쿼리를 DBMS가 미리 컴파일하여 대기하므로 이후, 인자에 대해서는 쿼리가 아닌 단순 문자열로 인식하기 때문에 안전하다.
