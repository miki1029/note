## 조건 분기 : CASE

* 단순 CASE 식, 검색 CASE 식(검색식이 단순식의 기능을 포함)

```sql
-- 검색 CASE 식의 구문
CASE WHEN [평가식] THEN [식]
     WHEN [평가식] THEN [식]
     WHEN [평가식] THEN [식]
     ...
     ELSE [식]
END
-- 평가식은 WHERE 구에 작성하는 방법과 동일
```

```sql
SELECT name, address
	CASE WHEN address = '서울시' THEN '경기'
		 WHEN address = '인천시' THEN '경기'
		 WHEN address = '부산시' THEN '영남'
		 WHEN address = '속초시' THEN '관동'
		 WHEN address = '서귀포시' THEN '호남'
		 ELSE NULL END AS district
FROM Address;
```

## 집합 연산

### 합집합 : UNION

```sql
-- 합집합을 조회(중복 제거)
SELECT * FROM Address
UNION
SELECT * FROM Address2;

-- 중복 포함
SELECT * FROM Address
UNION ALL
SELECT * FROM Address2;
```

### 교집합 : INTERSECT

```sql
SELECT * FROM Address
INTERSECT
SELECT * FROM Address2;
```

### 차집합 : EXCEPT (Oracle : MINUS)

```sql
SELECT * FROM Address
EXCEPT
SELECT * FROM Address2;
```

## 윈도우 함수

```sql
[집약 함수] + OVER([PARTITION BY|ORDER BY] 필드)
```

* PARTITION BY
	* GROUP BY(자르기+집약)에서 자르기 기능만 있는 것
	* GROUP BY와 비슷한 결과가 나타나지만 결과 레코드 수와 동일한 수의 결과가 나타난다.
* 윈도우 함수로 사용할 수 있는 함수
	* 일반 함수 : COUNT, SUM
	* 전용 함수
		* RANK
			* 지정된 키로 레코드에 순위를 붙이는 함수
			* 같은 숫자는 같은 순위로 표시하고 이후 건너뜀(공동 3등 이후 5등)
		* DENSE_RANK
			* RANK에서 건너뛰는 작업 없이 순위를 구함
		* ROW_NUMBER

```sql
SELECT address, COUNT(*) OVER(PARTITION BY address)
FROM Address;

SELECT name, age, RANK() OVER(ORDER BY age DESC) AS rnk
FROM Address;
```

```sql
-- 성별 별로 나이 순위(건너뛰기 있게)를 매기는 구문
SELECT name, sex, age, RANK() OVER(PARTITION BY sex ORDER BY age DESC) AS rnk
```

## UNION을 사용한 조건 분기

* 내부적으로 여러 개의 SELECT 구문으로 실행하는 실행 계획으로 해석되기 때문에 I/O 비용이 크게 늘어난다.

```sql
-- 2001년까지는 세금이 포함되지 않은 가격
-- 2002년부터는 세금이 포함된 가격
SELECT item_name, year, price_tax_ex AS price
FROM Items
WHERE year <= 2001
UNION ALL
SELECT item_name, year, price_tax_in AS price
FROM Items
WHERE year >= 2002;
```

* UNION ALL : 조건이 배타적이므로 중복된 레코드가 발생하지 않음
* 성능 문제점 발생 : 테이블 풀 스캔이 두 번 일어난다.(인덱스 스캔이 일어나는 상황이라면 얘기가 달라질 수도 있음)

### WHERE 구에서 조건 분기를 하는 사람은 초보자

```sql
SELECT item_name, year,
	CASE WHEN year <= 2001 THEN price_tax_ex
		 WHEN year >= 2002 THEN price_tax_in END AS price
FROM Items;
```

* 테이블 풀 스캔이 한 번 일어남으로써 UNION보다 성능이 좋아진다.
