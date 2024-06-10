# 데이터베이스 - 효과적인 쿼리 저장
## 개요
> 방대한 데이터가 있을때 효과적으로 쿼리를 날리는 방법에 대해 정리하기

## 방법
1. `SELECT`시 꼭 필요한 칼럼만 불러와야한다.
> 많은 필드 값을 불러올수록 DB는 더 많은 로드를 부담하게 됨
```sql
SELECT * FROME movie;
SELECT id FROM movie;
```

2. 조건 부여시, 가급적이면 기존 DB값에 별도의 연산을 걸지 않는 것이 좋다.

> full table scan -> 수식을 건 뒤, 조건 충족 여부를 판단
> r,value가 가지고있는 index를 그대로 사용하기 때문에 탐색필요 X

```sql
-- Inefficient
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
FROM movie m 
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE FLOOR(r.value/2) = 2 
GROUP BY m.id;
-- Improved
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
FROM movie m 
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE r.value BETWEEN 4 AND 5 
GROUP BY m.id;
```

3. LIKE 사용시 와일드카드 문자열(%)는 String 앞부분에 배치하지 않는 것이 좋다.
> 2번과 같은원리
value LIKE "%"는 -> full table scan
```sql
-- Inefficient
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value LIKE "%Comedy"  
GROUP BY g.value;
-- Improved(1): value IN (...)
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value IN ("Romantic Comedy", "Comedy") 
GROUP BY g.value;
-- Improved(2): value = "..."
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value = "Romantic Comedy" OR g.value = "Comedy"
GROUP BY g.value;
-- Improved(3): value LIKE "...%"
-- 모든 문자열을 탐색할 필요가 없어, 가장 좋은 성능을 내었습니다
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value LIKE "Romantic%" OR g.value LIKE "Comed%"
GROUP BY g.value;
```

4. SELECT DISTINCT, UNION DISTINCT와 같이 중복 값을 제거하는 연산은 최대한 사용하지 않아야한다.
> 중복 값을 제거하는 연산은 많은 시간이 걸림 따라서 `EXITS`를 활용하는 방법이 좋다.
```sql
FROM movie m  
INNER JOIN genre g 
ON m.id = g.movie_id;
-- Improved
SELECT m.id, title 
FROM movie m  
WHERE EXISTS (SELECT 'X' FROM rating r WHERE m.id = r.movie_id);
```

5. 같은 내용의 조건이라면, GROUP BY 연산시 HAVING 보다 WHERE절을 사용하는 것이 좋다.
> 쿼리의 실행순서에서 WHERE절은 HAVING 보다 먼저실행함 따라서 WHERE절로 크기를 줄이고 GROUP BY에서 다뤄야하는 데이터가 작아지므로 효과적인 연산이 가능
```sql
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
FROM movie m  
INNER JOIN rating r 
ON m.id = r.movie_id 
GROUP BY id 
HAVING m.id > 1000;
-- Improved
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
FROM movie m  
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE m.id > 1000
GROUP BY id ;
```

6. 3개 이상의 테이블을 INNER JOIN을 할때는 크기가 가장 큰 테이블을 `FROM` 절에 배치, INNER JOIN절에는 남은 테이블을 작은 순서대로 배치
> 항상 통용되지는 않는다
```sql
-- Query (A)
SELECT m.title, r.value rating, g.value genre 
FROM rating r 
INNER JOIN genre g 
ON g.movie_id = r.movie_id  
INNER JOIN movie m 
ON m.id = r.movie_id;
-- Query (B)
SELECT m.title, r.value rating, g.value genre 
FROM rating r 
INNER JOIN movie m
ON r.movie_id = m.id 
INNER JOIN genre g 
ON r.movie_id = g.movie_id;
```
7. 자주 사용하는 데이터 형식에 대해서는 미리 전처리된 테이블을 따로 보관/관리하는 것도 좋다.
- RDBMS의 원칙에 어긋나는 측면이 있고, DB의 실시간성을 반영하지 못할 가능성이 높기 때문에, 대부분 운영계보다는 분석계에서 더 많이 사용되곤 합니다. 
- 예를 들면 사용자에 의해 발생한 Log 데이터 중에서 필요한 Event만 모아서 따로 적재해두는 것, 혹은 핵심 서비스 지표를 주기적으로 계산해서 따로 모아두는 것 등이 가장 대표적으로 볼 수 있는 사례입니다.

출처: https://medium.com/watcha/%EC%BF%BC%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94-%EC%B2%AB%EA%B1%B8%EC%9D%8C-%EB%B3%B4%EB%8B%A4-%EB%B9%A0%EB%A5%B8-%EC%BF%BC%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-7%EA%B0%80%EC%A7%80-%EC%B2%B4%ED%81%AC-%EB%A6%AC%EC%8A%A4%ED%8A%B8-bafec9d2c073