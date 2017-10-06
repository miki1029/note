## 집약

* 집약 함수
	* COUNT
	* SUM
	* AVG
	* MAX
	* MIN
* SELECT 구에 입력할 수 있는 것
	* 상수
	* GROUP BY 구에서 사용한 집약 키
	* 집약 함수
* GROUP BY 집약 조작
	* HASH GROUP BY
		* 최근 SORT보다 더 많이 사용됨
		* 특히 유일성이 높으면 더 효율적으로 동작함
	* SORT GROUP BY
	* 충분한 워킹 메모리 필요(스왑이 발생하면 매우 느려짐 : TEMP 영역)
	* 심지어 TEMP 영역마저 다 써버리면 비정상적 종료가 될 수 있다.

## 자르기

### GROUP BY : 자르기 + 집약

```sql
SELECT CASE WHEN age < 20 THEN '어린이'
			WHEN age BETWEEN 20 AND 69 THEN '성인'
			WHEN age >= 70 THEN '노인'
			ELSE NULL END AS age_class,
		COUNT(*)
FROM Persons
GROUP BY CASE WHEN age < 20 THEN '어린이'
			  WHEN age BETWEEN 20 AND 69 THEN '성인'
			  WHEN age >= 70 THEN '노인'
			  ELSE NULL END;
```

* 이렇게 SELECT문과 GROUP BY에 같은 CASE문을 적어 원하는대로 자를 수 있다.
* 일부 DBMS는 age_class 별칭을 GROUP BY 구에 적는 것을 허용한다.(PostgreSQL, MySQL)

### PARTITION BY : 자르기

```sql
SELECT name, age,
		CASE WHEN age < 20 THEN '어린이'
			 WHEN age BETWEEN 20 AND 69 THEN '성인'
			 WHEN age >= 70 THEN '노인'
			 ELSE NULL END AS age_class,
		RANK() OVER(PARTITION BY
			CASE WHEN age < 20 THEN '어린이'
			     WHEN age BETWEEN 20 AND 69 THEN '성인'
			     WHEN age >= 70 THEN '노인'
			     ELSE NULL END
			     ORDER BY age) AS age_rank_in_class
FROM Persons
ORDER BY age_class, age_rank_in_class;
```

* 집약을 하지 않으므로 조회 레코드 수만큼 출력된다.
