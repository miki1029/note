# 실행 계획

## 실행 계획 확인

* Oracle : ```set autotrace traceonly```
* Microsoft SQL Server : ```SET SHOWPLAN_TEXT ON```
* DB2 : ```EXPLAIN ALL WITH SNAPSHOT FOR SQL 구문```
* PostgreSQL : ```EXPLAIN SQL 구문```
* MySQL : ```EXPLAIN EXTENDED SQL 구문```

## 실행 계획 출력 포맷

* Name : 조작 대상 객체
* Operation : 객체에 대한 조작의 종류
* Rows : 조작 대상이 되는 레코드 수 (JIT가 아니라면 카탈로그에서 값을 가져오기 때문에 실제 값과 차이가 발생할 수 있음)

## 실행 계획 예시

* 테이블 풀 스캔
	* Name : 테이블명
	* Operation : TABLE ACCESS FULL
	* Rows : 테이블 전체 레코드 수
* 인덱스 스캔 (id 조회)
	* Rows : 1
	* Operation | Name
		* TABLE ACCESS BY INDEX ROWID | 테이블명
		* INDEX UNIQUE SCAN | 인덱스명

<img src="https://github.com/miki1029/note/raw/master/sql/img/scan_time.jpg" width="500px"/>

* 간단한 테이블 결합
	* 실행 계획이 상당히 복잡해지므로 지연이 발생할 가능성이 높다.
	* 세 가지 알고리즘이 존재 (Operation)
		* Nested Loops : 한쪽 테이블을 읽으면서 레코드 하나마다 결합 조건에 맞는 레코드를 다른 쪽 테이블에서 찾는 방식
		* Sort Merge : 결합 키로 레코드를 정렬하고, 순차적으로 두 개의 테이블을 결합하는 방법. 정렬을 수행할 때 워킹 메모리를 사용한다.
		* Hash : 결합 키값을 해시값으로 맵핑하는 방법. 해시 테이블을 만들어야 하므로 작업용 메모리 영역을 필요로 한다.
