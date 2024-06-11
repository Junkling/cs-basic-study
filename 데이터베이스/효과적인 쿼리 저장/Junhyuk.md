# **효과적인 쿼리 저장**

**1. Select 시 에는 꼭 필요한 컬럼만 불러와야 한다**

```sql
//나쁜 예
SELECT *
FROM member m

//개선
SELECT m.id
FROM member m
```

**2. 조건에 별도의 연산을 걸지 않는 것이 좋다**

```sql
-- 나쁜
SELECT m.id, COUNT(s.id) s_count 
FROM member m 
INNER JOIN subject s 
ON m.id = s.member_id 
WHERE FLOOR(s.grade/10) = 7 
GROUP BY m.id;

-- 개선
SELECT m.id, COUNT(s.id) s_count 
FROM movie m 
INNER JOIN subject s 
ON m.id = r.member_id 
WHERE s.grade BETWEEN 70 AND 80 
GROUP BY m.id;
```

**3. LIKE사용 시 와일드카드 문자열(%)을 String 앞부분에는 배치하지 않는 것이 좋다**. **(순차탐색을 사용함으로 성능이 떨어짐)** 

- 예를 들어 SpringBoot, SpringFramework, StudySpring 를 조건문으로 조회한다고 했을 때 **Like %Spring%** 과 같이 작성하는 것 보다 Where In 키워드로 최대한 특정하거나 최대한 루프를 덜 돌도록 %연산을 사용하는 것이 좋다.
    - %가 앞에 붙으면 전체를 탐색함으로 성능이 떨어진다.

**4, SELECT DISTINCT, UNION DISTINCT와 같은 중복값을 제거하는 함수는 최소화 한다.**

```sql
//나쁜 예
SELECT DISTINCT m.id, name
FROM member m  
INNER JOIN subject s
ON m.id = s.member_id;

//개선
SELECT m.id, title 
FROM member m  
WHERE EXISTS (SELECT 'e' FROM subject s WHERE m.id = s.member_id);
```

**5. 같은 내용의 조건이라면, GROUP BY 연산 시에는 가급적 HAVING 보다는 WHERE 절을 사용하는 것이 좋다**

- where 절은 having보다 먼저 실행됨으로 데이터 크기를 작게 만들어 놓을 수 있기 때문

```sql
//나쁜 예
SELECT m.id, COUNT(s.id) AS subject_count, AVG(s.grade) AS grade_avarge
FROM member m  
INNER JOIN subject s
ON m.id = s.member_id 
GROUP BY id 
HAVING m.id > 10;

//개선
SELECT m.id, COUNT(s.id) AS subject_count, AVG(s.grade) AS grade_avarge
FROM member m  
INNER JOIN subject s
ON m.id = s.member_id 
WHERE m.id > 10
GROUP BY id ;
```

**6. 3개 이상의 테이블을 inner join 할 때는, 크기가 가장 큰 테이블을 from 절에 배치하고, inner join 절에는 남은 테이블을 작은 순서대로 배치하는 것이 좋다**

- Join 문이 많아질수록 성능이 떨어짐으로 inner join시 큰 테이블을 from에 배치해야 유리