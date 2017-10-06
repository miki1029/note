# UNION과 CASE 조건 분기

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


## 집계 대상으로 조건 분기

* 인구 테이블

|지역 이름(prefecture)|성별(sex)|인구(pop)|
|:-:|:-:|:-:|
|...|...|...|

* 원하는 결과

|지역 이름|남자 인구수|여자 인구수|
|:-:|:-:|:-:|
|...|...|...|

* UNION을 사용한 방법
	* 남성 인구를 구하고 여자 인구를 구한 뒤 merge
	* 인덱스 스캔이 아니라면 풀 스캔 2번 일어나므로 비효율적

```sql
SELECT prefecture, SUM(pop_men) AS pop_men, SUM(pop_wom) as pop_wom
FROM (SELECT prefecture, pop AS pop_men, NULL AS pop_wom
	FROM Population
	WHERE sex = '1'
	UNION
	SELECT prefecture, NULL AS pop_men, pop AS pop_wom
	FROM Population
	WHERE sex = '2') TMP
GROUP BY prefecture;	
```

* CASE를 사용한 방법
	* 식도 간단해진다.
	* 풀 스캔이 1회로 감소

```sql
SELECT prefecture,
	SUM(CASE WHEN sex = '1' THEN pop ELSE 0 END) AS pop_men,
	SUM(CASE WHEN sex = '2' THEN 0 ELSE pop END) AS pop_wom
FROM Population
GROUP BY prefecture;
```

## 집약 결과로 조건 분기

|직원ID(emp_id)|팀ID(team_id)|직원 이름(emp_name)|팀(team)|
|:-:|:-:|:-:|:-:|
|...|...|...|...|

* 소속된 팀이 1개이면 팀 이름 출력
* 2개이면 2개 겸무 출력
* 3개이면 3개 이상 겸무를 출력

|직원 이름|팀|
|:-:|:-:|
|...|...|

* UNION을 사용한 방법
	* 풀 스캔 3번 일어남
	* GROUP BY의 HASH 연산도 3번 일어남

```sql
SELECT emp_name, MAX(team) AS team
FROM Employees
GROUP BY emp_name
HAVING COUNT(*) = 1
UNION
SELECT emp_name, '2개 겸무' AS team
FROM Employees
GROUP BY emp_name
HAVING COUNT(*) = 2
UNION
SELECT emp_name, '3개 이상을 겸무' AS team
FROM Employees
GROUP BY emp_name
HAVING COUNT(*) >= 3;
```

* CASE를 사용한 방법
	* 테이블 스캔, HASH 연산 모두 1회로 줄어듦

```sql
SELECT emp_name,
	CASE WHEN COUNT(*) = 1 THEN MAX(team)
		 WHEN COUNT(*) = 2 THEN '2개 겸무'
		 WHEN COUNT(*) >= 3 THEN '3개 이상을 겸무'
	END AS team
FROM Employees
GROUP BY emp_name;	
```

# UNION이 필요한 경우

## UNION을 사용할 수밖에 없는 경우

* 테이블이 다른 경우 (CASE 식도 사용 가능하나 필요 없는 결합이 발생해서 성능적으로 어떤 것이 더 좋은지 확인이 필요하다.)

## UNION이 성능적으로 더 좋은 경우

* 인덱스와 관련된 경우
	* UNION을 사용했을 때 좋은 인덱스를 사용하지만, 이외의 경우 테이블 풀 스캔이 발생하는 경우
	* WHERE 구문에서 OR을 사용하면 해당 필드에 부여된 인덱스를 사용할 수 없다.

```sql
-- UNION
-- 3번의 인덱스 스캔 (데이터 수가 적으면 풀스캔이 일어날 가능성도 있긴 함)
-- 데이터 수가 많아질 수록 아래 3개보다 유리하다.
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE date_1 = '2013-11-01'
AND flg_1 = 'T'
UNION
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE date_2 = '2013-11-01'
AND flg_2 = 'T'
UNION
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE date_3 = '2013-11-01'
AND flg_3 = 'T';

-- OR
-- 한 번의 풀스캔 (인덱스 사용 불가)
-- DBMS에 따라서 UNION을 사용할 때와 같은 실행 계획을 만들기도 한다.
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE (date_1 = '2013-11-01' AND flg_1 = 'T')
	OR (date_2 = '2013-11-01' AND flg_2 = 'T')
	OR (date_3 = '2013-11-01' AND flg_3 = 'T')

-- IN
-- 실행 계획은 OR의 경우와 동일
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE ('2013-11-01', 'T') IN ((date_1, flg_1), (date_2, flg_2), (date_3, flg_3));

-- CASE
-- 실행 계획은 OR의 경우와 동일
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
FROM ThreeElements
WHERE CASE WHEN date_1 = '2013-11-01' THEN flg_1
		   WHEN date_2 = '2013-11-01' THEN flg_2
		   WHEN date_3 = '2013-11-01' THEN flg_3
		   ELSE NULL END = 'T';
```
