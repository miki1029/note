# 기본 키 할당 전략

## IDENTITY 전략

* MySQL, PostgreSQL, SQL Server, DB2
* 기본 키를 insert 해야 알 수 있기 때문에 ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) **`persist() 호출시 곧바로 insert`** 가 일어난다.

```java
@GeneratedValue(strategy = GenerationType.IDENTITY)
```

## SEQUENCE 전략

* Oracle, PostgreSQL, DB2, H2
* persist 호출시에 시퀀스 select가 일어나고 **transaction commit시에 insert**가 일어난다.

### allocationSize

* 성능 최적화를 위해 디폴트 값이 50
* 1보다 클 경우 jpa가 메모리에 캐시하고 있기 때문에 시퀀스 select가 일어나지 않을 수도 있다.
* 시퀀스가 한 번에 크게 증가하는 상황이 부담스럽고 insert 성능이 중요하지 않다면 allocationSize = 1 로 설정해서 사용하는 것이 나을 수 있다.
* **allocationSize는 hibernate.id.new\_generator\_mappings 프로퍼티 설정에 따라서 동작이 달라진다.**

### hibernate.id.new\_generator\_mappings = true

* **hibernate 5 버전에서 디폴트 설정 값**이다.
* 하이버네이트의 새로운 시퀀스 할당 전략이다.
	* 하위버전 호환이 되지 않을 수 있으므로 주의
* allocationSize 만큼 시퀀스 값을 한 번에 증가시키고 메모리에서 시퀀스를 할당하는 개념
	* allocationSize = 50 이면 반드시 DB 시퀀스도 increment by 50으로 되어 있어야 한다.
  
### hibernate.id.new\_generator\_mappings = false

* hibernate 5 버전에서 디폴트 설정 값은 true이지만 ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) **`spring-boot-autoconfigure를 사용하면 디폴트 값이 false`** 이다.
* 구 버전 하이버네이트가 동작했던 방식이다.
	* 새로운 프로젝트라면 이 값을 false로 사용할 이유가 없다. 반드시 true로 설정하자.
* 시퀀스는 항상 increment by 1로 사용
	* allocationSize = 50 일때, 시퀀스가 1이면 1~50, 2이면 51~100을 할당

```java
// @SequenceGenerator는 class, @Id 필드 아무대나 붙여도 되고 @GeneratedValue는 @Id 필드에 붙이면 된다.
@SequenceGenerator(
	name = "MYTABLE_SEQ_GENERATOR",
	sequenceName = "MYTABLE_SEQ",
	initialValue = 1, allocationSize = 50)
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "MYTABLE_SEQ_GENERATOR")
```

## TABLE 전략

* 데이터베이스 시퀀스를 흉내내는 전략이므로 모든 DB에서 사용 가능
* 기본적으로 SEQUENCE와 동일하다고 보면 된다.
* 단, 시퀀스는 조회하면서 값이 증가하지만 ![#f03c15](https://placehold.it/15/f03c15/000000?text=+)**` 테이블 전략은 select, update 두 번의 쿼리`**가 발생한다.
	* 그렇다 하더라도 시퀀스와 마찬가지로 allocationSize를 통해 최적화할 수 있다.
	* select for update를 통해 락을 잡는다. : <http://www.dator.co.kr/hotshin/textyle/236147>

```sql
CREATE TABLE MY_SEQUENCES (
	SEQUENCE_NAME VARCHAR(255) NOT NULL,
	NEXT_VAL BIGINT,
	PRIMARY KEY (SEQUENCE_NAME)
)
```

```java
// @ TableGenerator는 class, @Id 필드 아무대나 붙여도 되고 @GeneratedValue는 @Id 필드에 붙이면 된다.
@TableGenerator(
	name = "MYTABLE_SEQ_GENERATOR",
	table = "MY_SEQUENCES",
	pkColumnValue = "MYTABLE_SEQ",
	allocationSize = 1)
@GeneratedValue(strategy = GenerationType.TABLE, generator = "MYTABLE_SEQ_GENERATOR")
```
