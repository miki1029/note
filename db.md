# Database

<http://www.mysqltutorial.org>  
<https://sqlbolt.com>  

# BigData

[쉽게 배우는 하둡 에코 시스템 2.0](http://blrunner.com/99)  
[DynamoDB와 HBase 비교 분석하기](http://blog.recopick.com/33)  

# Oracle
테이블 스페이스 생성
```sql
create tablespace prodsonar datafile '/home1/irteam/oracle/oradata/XE/prodsonar.dbf' size 1024m;
```

테이블 스페이스 조회
```sql
select tablespace_name, bytes/1024/1024 MB , file_name from dba_data_files;
select tablespace_name, file_name, online_status from dba_data_files where tablespace_name='USERS';
```
<http://oracleocm.tistory.com/entry/오라클-테이블스페이스-위치-변경>  
<http://goalker.tistory.com/95>  

# MongoDB

* <https://marsettler.com/mongodb/mongodb-study-week-2/>
* 일관성 : <http://mongodb.citsoft.net/?page_id=6>
