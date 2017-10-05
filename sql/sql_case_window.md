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
