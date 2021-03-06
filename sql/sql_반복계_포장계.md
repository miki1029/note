# 반복계 vs 포장계

## 반복계

### 반복계의 단점

* <처리 횟수> * <한 회에 걸리는 처리 시간> : 처리 시간이 선형적으로 증가

<img src="https://github.com/miki1029/note/raw/master/sql/img/process_time.jpg" width="500px"/>

* SQL 실행의 오버 헤드
	* 전처리
		* SQL 구문을 네트워크로 전송 : 같은 데이터센터라면 오버헤드가 거의 없음
		* 데이터베이스 연결 : 커넥션 풀 이용하면 빠름
		* **SQL 구문 파스**
			* 느린 부분은 0.1초 ~ 1초까지도 걸림
			* 특히 반복계 코드라면 이러한 현상이 반복적으로 나타나게 됨
		* **SQL 구문의 실행 계획 생성 또는 평가**
	* 후처리
		* 결과 집합을 네트워크로 전송 : 같은 데이터센터라면 오버헤드가 거의 없음
* 병렬 분산이 어려움
	* 반복계 SQL 구문은 대부분 단순해서 1회 SQL 구문이 접근하는 데이터양이 적다.
* 데이터베이스 진화로 인한 혜택을 받을 수 없다.
	* DBMS의 버전이 오를수록 옵티마이저는 더 효율적으로 실행 계획을 세울 수 있다.
	* 대규모 데이터를 다루는 복잡한 SQL 구문을 빠르게 하기 위한 노력이 계속 되고 있다.
	* 단순한 SQL 구문과 같은 가벼운 처리를 빠르게 만드는 것은 안중에도 없다.

### 반복계를 빠르게

* 반복계를 포장계로 다시 작성
	* 좋은 방법이지만 이제와서 수정하기 어려울지도 모름
* 각각의 SQL을 빠르게 수정
	* 하지만, 충분히 단순하다면 더이상 튜닝이 어려움
	* INSERT의 경우 실행계획을 조작하는 튜닝은 할 수 없다. 커밋 단위를 확장하는 정도만 가능한데, 이 조차도 트랜잭션 사용 방식에 따라서 사용하지 못할 수도 있고, 벌크 INSERT를 지원하지 않는 DBMS에서는 사용 불가능하다.
* 다중화 처리
	* CPU, 디스크와 같은 리소스 여유가 있고, 처리를 나눌 수 있는 키가 명확한 경우

### 반복계의 장점

* 실행 계획의 안정성
	* 실행 계획에 변동 위험이 거의 없다.
* 예상 처리 시간의 정밀도
	* <처리 시간> = <처리 횟수> * <한 회에 걸리는 처리 시간>
* 트랜잭션 제어가 편리
	* 미세한 트랜잭션 제어
	* 오류가 발생해도 중간에 커밋한 부분부터 다시 처리를 실행하면 된다.

## 포장계

* CASE 식
* 윈도우 함수

```sql
INSERT INTO Sales2
SELECT company, year, sale,
	CASE SIGN(sale - MAX(sale)
		OVER (PARTITION BY company
			  ORDER BY year
			  ROWS BETWEEN 1 PRECEDING AND 1 PRECEDING))
	WHEN 0 THEN '='
	WHEN 1 THEN '+'
	WHEN -1 THEN '-'
	ELSE NULL END AS var
FROM Sales;	
```

* SIGN : 숫자 자료형을 받아 음수면 -1, 양수면 1, 0이면 0을 리턴하는 함수
* ROWS BETWEEN 1 PRECEDING AND 1 PRECEDING : 현재 레코드에서 1개 전부터 1개 전까지 -> 대상 범위를 1개로 제한
* MAX는 그냥 집계함수를 써야되서 쓴 듯
* 185p, 192p
