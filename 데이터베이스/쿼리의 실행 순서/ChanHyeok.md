### 쿼리의 실행 순서

```sql
SELECT [DISTINCT] 필드명
FROM 테이블명
WHERE 조건
GROUP BY 컬럼명
HAVING 조건
ORDER BY 컬럼명 [ASC || DESC]
```

*(SELECT 쿼리 기본 구조)*

***'FROM(AND JOIN)' -> 'WHERE' -> 'GROUP BY' -> 'HAVING' -> 'SELECT' -> 'ORDER BY'***

SELECT 쿼리는 작성되는 순서와 실행되는 순서가 다름

특히 JOIN, WHERE, GROUP BY, HAVING 단계에서는 쿼리를 어떻게 작성하느냐에 따라 성능적 차이가 발생
<br></br>
**1. FROM(AND JOIN)**

- 조회할 대상 테이블을 지정하고 *(INDEX를 사용하지 않는다는 가정 )* 해당 테이블의 모든 행*(row)*을 가져옵니다.
- 이후 JOIN을 실행하여 조회 대상 데이터를 하나의 가상 테이블로 결합합니다.<br></br>

**2. WHERE**

- FROM 절에서 가져온 테이블의 특정 조건을 만족하는 행만 선택합니다.
- WHERE 절에서는 조건을 정의하고, 각 행*(row)*이 정의된 조건을 만족하는지를 평가하여 참*(TRUE)* 또는 거짓*(FALSE)*을 결정합니다.
- 그리고 정의된 조건을 만족*(결과가 TRUE 인)*하는 행만 결과 집합에 포함하여 반환합니다.<br></br>

**3. GROUP BY**

- 특정 열을 기준으로 그룹을 만듭니다.
- 필요한 경우 그룹화된 결과에 대해 그룹 함수*(COUNT, SUM, AVG, MIN, MAX 등)*를 적용하여 그룹별 집계를 수행할 수 있습니다.<br></br>

**4. HAVING**

- 그룹에 대한 조건을 지정합니다.
- **WHERE 절**이 각 **행*(row)* 단위로 조건을 비교**하는 반면, **HAVING 절**은 **그룹핑된 그룹 단위로 조건을 비교**합니다.
    - 만약 HAVING 절에 사용하는 조건을 WHERE 절에 사용해도 무방하다면 WHERE 절에 사용하는 것이 좋다
      →WHERE 절을 통해 필터링된 행만을 그룹화하여 집계를 수행하기 때문에 처리할 데이터 양이 줄어들어 성능 향상<br></br>

**5. SELECT**

- 조회할 열을 선택<br></br>

**6. ORDER BY**

- 조회할 열을 정했다면 행의 순서를 어떻게 보여줄지 **정렬**
- 기본적으로 오름차순*(ASC)*으로 정렬되며, 지정 시 내림차순*(DESC)*으로도 정렬<br></br>

**7. LIMIT**

- 결과 중 몇개의 행을 보여줄 지 선택.<br></br>

** +  DISTINCT**

- SELECT와 ORDER BY 사이에 적용되며, **중복된 행을 제거**하는 데 사용됩니다.
- 'SELECT DISTINCT'와 같은 형식으로 사용, 중복된 값을 가지는 행을 하나의 행으로 압축<br></br>


**실행순서가 중요한 이유**
1. **문법**

    - **OrderBy 절에서 Alias 사용**
    
    ```sql
    SELECT CONCAT(first_name, last_name)AS full_name
    FROMuserORDERBY full_name;
    ```
    
    -ORDER BY 절은 SELECT 절보다 뒤에 실행되기 때문에 SELECT 절의 결과를 사용할 수 있다.
    
    - **Where 절에서 Alias 사용**
    
    ```sql
    SELECT CONCAT(first_name, last_name)AS full_name
    FROMuserWHERE full_name = 'VioletBeach';
    ```
    
    Where 절에서는 SELECT 절보다 먼저 실행된다.
    
    즉, WHERE 절은 FROM 절의 결과를 가지고 필터링을 하는 용도이지 SELECT문 에서 사용한 AS를 활용할 수 없다. 그래서 해당 쿼리는 에러가 발생한다.
    
    WHERE 절에서 Alias를 사용하려다가 원치 않는 결과를 받는다거나, ORDER BY 절에서 SELECT 절에서 사용된 함수를 또 호출해서 자원이 낭비되는 이슈를 막으려면 실행 순서에 대한 이해가 필요하다.<br></br>

2. **성능**

    - **JOIN할 테이블이 많고 레코드도 매우 많다면** 수 많은 레코드를 가지고 `WHERE`, `GROUP BY`, `ORDER BY` 등을 수행하게 된다.

    - **SQL 실행 순서를 이해**한다면 `LIMIT`을 포함해 먼저 ID만을 필터링한 후에 복잡한 `JOIN`을 포함한 나머지 컬럼 조회는 필터링된 레코드의 ID만을 가지고 **후속 쿼리**에서 실행할 수도 있을 것이다.

    - 그러면 실제로 검색할 때 데이터의 행, 열 모두 크기가 매우 작아지므로 부하가 감소된다. 무거운 쿼리 1개를 가벼운 쿼리 2개로 나눴지만, 실행 시간이나 부하는 10배 개선될 수도 있다.

   - SQL 실행 순서를 이해하면 **쿼리의 성능이 낭비되는 지점이 잘 보이고 튜닝을 할 수 있게 된다.**

**팁**

**SQL을 실행 순서대로 작성**

- SELECT -> FROM -> WHERE 순으로 작성 X

- FROM -> WHERE -> SELECT 순으로 작성

→ 중간에 성능적으로 튜닝할 수 있는 요소를 발견할 가능성이 높아진다

<br></br><br></br><br></br>
출처
- https://jaehoney.tistory.com/191
- https://devparker.tistory.com/96