# 메모리 버퍼

<img src="https://github.com/miki1029/note/raw/master/sql/img/data_cache_log_buffer.jpg" width="700px"/>
<img src="https://github.com/miki1029/note/raw/master/sql/img/working_memory.jpg" width="700px"/>

* 데이터 캐시
	* 디스크에 있는 데이터의 일부를 메모리에 유지
	* 손실되어도 다시 로드하면 되므로 큰 문제 없음
	* 디폴트 버퍼 사이즈 큼 (로그 버퍼보다)
	* MySQL 메뉴얼에서 서버가 DB 전용이라면 메모리의 80%를 버퍼 풀로 할당할 것을 권장
* 로그 버퍼
	* 갱신 처리에 대한 버퍼를 두고 있다가 커밋할 때 디스크에 씀
	* 손실될 경우 비즈니스 로직에 큰 문제가 발생할 수 있음
	* 커밋 시점에 반드시 갱신 정보를 로그 파일에 씀으로써 정합성 유지
	* 디폴트 버퍼 사이즈 작음 : 조회에 비해 자주 일어나지 않는다고 가정
	* 조회보다 갱신이 많은 시스템이라면 튜닝 필요함
* 워킹 메모리
	* 정렬(order by 절, 집합 연산, 윈도우 함수 등) 또는 해시(테이블 등의 결합, 최근에는 group by 절) 관련 처리
	* 정렬 또는 해시가 필요한 때 사용되고, 종료되면 해제되는 임시 영역
	* 데이터가 워킹 메모리 사이즈보다 클 경우 디스크를 사용하며 스왑 현상이 발생함
	* doc : [Oracle](http://docs.oracle.com/cd/B28359_01/server.111/b28318/memory.htm), [PostgreSQL](http://www.postgresql.org/docs/9.4/static/runtime-config-resource.html), [MySQL](http://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_sort_buffer_size)
	* 워킹 메모리가 부족할 때 사용하는 임시적인 영역에 대한 명칭(디스크)
		* Oracle : TEMP Tablespace
		* Microsoft SQL Server : TEMPDB
		* PostgreSQL : pgsql_tmp
