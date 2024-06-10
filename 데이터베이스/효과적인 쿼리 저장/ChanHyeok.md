### 효과적인 쿼리 저장

**1. Select 시 에는 꼭 필요한 컬럼만 불러와야 한다**

```sql
SELECT * FROM member;

SELECT id FROM member;
```

- 많은 필드 값을 불러올수록 DB는 더 많은 로드를 부담하게 되기 때문
- 컬럼 중에 불필요한 값을 가진 필드가 있다면 과감히 제외하고, 꼭 필요한 열만 불러오는 것이 좋다<br></br>

**2.  조건 부여 시, 가급적이면 기존 DB값에 별도의 연산을 걸지 않는 것이 좋다**

```sql
-- 나쁜SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count
FROM movie m
INNER JOIN rating r
ON m.id = r.movie_id
WHERE FLOOR(r.value/2) = 2
GROUP BY m.id;

-- 개선SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count
FROM movie m
INNER JOIN rating r
ON m.id = r.movie_id
WHERE r.value BETWEEN 4 AND 5
GROUP BY m.id;
```

- between 4 and 5 = 4~5 로 명시 (상수로 주어 별도의 연산이 없게 하는 것)

![image](https://github.com/5dotseven/cs-basic-study/assets/118906074/4f284af7-5130-4473-8897-1c676977a1d6)

- **나쁜** **쿼리 -** Full Table Scan(순차접근=순차탐색)을 하면서 모든 Cell 값을 탐색하고, 수식을 건 뒤, 조건 충족 여부를 판단 = **O(n)**

- **개선** **쿼리 -** 기존에 r.value가 가지고 있는 index를 그대로 활용할 수 있기 때문에 모든 필드 값을 탐색할 필요가 없어 **나쁜쿼리** 대비 더 짧은 Running Time을 가질 수 있다 **= O(log n)**
- 많은 필드 값을 불러올수록 DB는 더 많은 로드를 부담하게 되기 때문
- 컬럼 중에 불필요한 값을 가진 필드가 있다면 과감히 제외하고, 꼭 필요한 열만 불러오는 것이 좋다<br></br>

**3 . LIKE사용 시 와일드카드 문자열(%)을 String 앞부분에는 배치하지 않는 것이 좋다**

- 2번과 같은 원리입니다. **Index**를 활용할 수 있는 value IN (...), value = "...", value LIKE "...%" 와는 다르게, value LIKE "%..."는 **Full Table Scan (순차 탐색)**을 활용합니다.
 - 따라서 같은 결과를 낼 수 있다면, value LIKE "%..."보다는 다른 형태의 조건을 적용하는 것이 바람직합니다.

예를 들면, 다양한 장르 중에서 Comedy와 Romantic Comedy를 추출하고 싶은 경우, LIKE "%Comedy"보다는, 다른 형태의 조건절을 사용하는 것이 효과적일 것입니다.

```sql
-- 나쁜SELECT g.value genre, COUNT(r.movie_id) r_cnt
FROM rating r
INNER JOIN genre g
ON r.movie_id = g.movie_id
WHERE g.value LIKE "%Comedy"
GROUP BY g.value;

-- 개선(1): value IN (...)SELECT g.value genre, COUNT(r.movie_id) r_cnt
FROM rating r
INNER JOIN genre g
ON r.movie_id = g.movie_id
WHERE g.value IN ("Romantic Comedy", "Comedy")
GROUP BY g.value;

-- 개선(2): value = "..."SELECT g.value genre, COUNT(r.movie_id) r_cnt
FROM rating r
INNER JOIN genre g
ON r.movie_id = g.movie_id
WHERE g.value = "Romantic Comedy" OR g.value = "Comedy"
GROUP BY g.value;

-- 개선(3): value LIKE "...%"-- 모든 문자열을 탐색할 필요가 없어, 가장 좋은 성능을 내었습니다SELECT g.value genre, COUNT(r.movie_id) r_cnt
FROM rating r
INNER JOIN genre g
ON r.movie_id = g.movie_id
WHERE g.value LIKE "Romantic%" OR g.value LIKE "Comed%"
GROUP BY g.value;
```

![image (1)](https://github.com/5dotseven/cs-basic-study/assets/118906074/10c8a8ed-09d4-4fcc-9a5c-d0896336147a)
개선(3) 의 경우 Romantic Comedy 경우 %Comedy 라고 하는 경우 해당 컬럼이 Comedy 로 끝나는지 **문자열을 거의 모두 돌게 될 수 있습니다**.

때문에 Romantic% 일 때를 **문자열을 모두 순환하지않으면,** **바로 OR 로 넘어가 Comed% 인지 확인하는 식으로 성능을 개선**시킬 수 있다<br></br>

**4. SELECT DISTINCT, UNION DISTINCT와 같이 중복 값을 제거하는 연산은 최대한 사용하지 않아야 한다**

- 중복값을 제거하는 연산은 많은 시간이 걸린다. 만약 불가피하게 사용해야 하는 상황이라면 distinct 연산을 대체하거나, 연산의 대상이 되는 테이블의 크기를 최소화 하는 방법을 고민할 필요가 있다.

가장 대표적인 대체방법으로는 **exists 를 활용**하는 방법이 있다

```sql
-- 나쁜SELECT DISTINCT m.id, title
FROM movie m
INNER JOIN genre g
ON m.id = g.movie_id;

-- 개선SELECT m.id, title
FROM movie m
WHERE EXISTS (SELECT 'X' FROM rating r WHERE m.id = r.movie_id);
```

![image (2)](https://github.com/5dotseven/cs-basic-study/assets/118906074/ed610f4a-0a54-48fd-9fc4-076919c0d62d)
5. 같은 내용의 조건이라면, GROUP BY 연산 시에는 가급적 HAVING 보다는 WHERE 절을 사용하는 것이 좋다

- 쿼리 실행 순서에서, where 절이 having 절 보다 먼저 실행 된다.

- 따라서 where 절로 미리 데이터 크기를 작게 만들면, grouby by 에서 다뤄야 하는 데이터 크기가 작아지기 때문에 보다 효율적인 연산이 가능하다

```sql
-- 나쁜SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating
FROM movie m
INNER JOIN rating r
ON m.id = r.movie_id
GROUP BY id
HAVING m.id > 1000;

-- 개선SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating
FROM movie m
INNER JOIN rating r
ON m.id = r.movie_id
WHERE m.id > 1000
GROUP BY id ;
```

![image (4)](https://github.com/5dotseven/cs-basic-study/assets/118906074/5dc8b4ef-9bec-40b4-96d4-f1b1e1632401)<br></br>
**6. 3개 이상의 테이블을 inner join 할 때는, 크기가 가장 큰 테이블을 from 절에 배치하고, inner join 절에는 남은 테이블을 작은 순서대로 배치하는 것이 좋다**

- inner join 과정에서 최소한의 combination 을 탐색하도록 from & inner join 의 순서를 배열하면 좋다 -> **항상 통용되지는 않는다**

- inner조인은 대부분 Query Planner에서 가장 효과적인 순서를 탐색해 inner join순서를 변경 -> 실행 시간 차이 거의 없다.


```sql
-- Query (A)SELECT m.title, r.value rating, g.value genre
FROM rating r
INNER JOIN genre g
ON g.movie_id = r.movie_id
INNER JOIN movie m
ON m.id = r.movie_id;

-- Query (B)SELECT m.title, r.value rating, g.value genre
FROM rating r
INNER JOIN movie m
ON r.movie_id = m.id
INNER JOIN genre g
ON r.movie_id = g.movie_id;
```

![image (5)](https://github.com/5dotseven/cs-basic-study/assets/118906074/b606aa32-9a8b-4435-8798-b6d30eea73a4)



7. 자주 사용하는 데이터의 형식에 대해서는 미리 전처리된 테이블을 따로 보관/관리하는 것도 좋습니다.

- RDMBS 의 원칙에 어긋나는 측면이 있고, DB의 실시간성을 반영하지 못 할 가능성이 높기 때문에, 대부분 운영계보다는 분석계에서 더 많이 사용

- Ex) 사용자에 의해 발생한 Log 데이터 중에서 필요한 Event 만 모아서 적재해두는 것, 핵심 서비스 지표를 주기적으로 계산해서 따로 모아두는 것