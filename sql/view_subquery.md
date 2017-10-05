## 뷰

* SELECT 구문을 저장하는 개념

```sql
CREATE VIEW [뷰 이름] ([필드 이름1], [필드 이름2], ...) AS SELECT 문
```

```sql
CREATE VIEW CountAddress (v_address, cnt)
AS
SELECT address, COUNT(*)
FROM Address
GROUP BY address;
```

```sql
SELECT v_address, cnt
FROM CountAddress;
```

## 익명 뷰(서브 쿼리)

```sql
SELECT v_address, cnt
FROM (SELECT address, COUNT(*)
	FROM Address
	GROUP BY address) AS CountAddress;
```

## 서브쿼리

```sql
SELECT name
FROM Address
WHERE name IN (SELECT name FROM Address2);
```
