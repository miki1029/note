## mysql driver의 serverTimezone의 역할

jpa에서 OffsetDateTime을 사용하면 이는 mysql driver 단에서 java.sql.Timestamp로 변환되어 사용됩니다.
즉, 현재 serverTimezone에 따라서 다르게 저장되는 부분은 jpa와 관계가 없으며 java.sql.Timestamp를 파라미터로 사용하는 PreparedStatement를 사용시 영향이 있습니다.

java.sql.Timestamp를 파라미터 바인딩하는 시점에 serverTimezone에 맞춰서 `yyyy-MM-dd HH:mm:ss` 포맷으로 변환하게 됩니다.
**즉, mysql driver가 java.sql.Timestamp의 변수 바인딩을 unix timestamp로 하지 않고 `yyyy-MM-dd HH:mm:ss`로 하기 때문에 timestamp type임에도 불구하고 timezone에 영향을 받게 되는 문제가 있는 것입니다.** (mysql driver만의 문제인 것인지는 잘 모르겠네요. mysql이 아닌 다른 driver에서 만약 java.sql.Timestamp의 변수 바인딩을 unix timestamp로 바인딩을 한다면 아마 문제가 없을 것 같습니다.)

심지어 이 과정에서 serverTimezone과 실제 mysql server의 timezone이 다르게 되면 unix timestamp가 손실될 수 있는 문제가 발생하게 됩니다.
**serverTimezone 파라미터는 반드시 mysql server timezone과 일치해야 하며, default가 mysql server timezone으로 설정됩니다.**
따라서 serverTimezone 파라미터는 아에 사용하지 않는 방향이 맞아 보입니다.

아래는 mysql driver 코드의 일부입니다.

```
// ClientPreparedQueryBindings.java
public void setTimestamp(int parameterIndex, Timestamp x, Calendar targetCalendar, int fractionalLength) {
	// ...

	this.tsdf = TimeUtil.getSimpleDateFormat(this.tsdf, "''yyyy-MM-dd HH:mm:ss", targetCalendar,
	                    targetCalendar != null ? null : this.session.getServerSession().getDefaultTimeZone());

	StringBuffer buf = new StringBuffer();
	buf.append(this.tsdf.format(x)); // 여기에서 serverTimezone과 mysql server timezone이 일치하지 않으면 unix timestamp를 잃게 됩니다.

	// ...
}
```

결론 (검증 필요...)
* serverTimezone 파라미터는 사용하지 않는다.
* mysql server는 각 국가의 timezone으로 실행한다.(사실 이 부분은 UTC로 실행해도 큰 문제는 없습니다. 다만 여러 국가을 위한 DB가 아니라면 각 국가별 timezone을 사용하는 것이 가독성이 좋을 것 같습니다.)
* app server는 어떤 타임존으로 띄우더라도 mysql로 timestamp 데이터 저장시에는 mysql server의 timezone으로 변환하여 저장하기 때문에 아무런 문제가 없다.

## utf8mb4

* <https://blog.lael.be/post/917>
* <https://blog.naver.com/seuis398/220851196727>

## type

* <https://honsal.blogspot.com/2017/04/mysql-java.html>

## index

* 커버링 인덱스 성능 : <https://gywn.net/2012/04/mysql-covering-index/>
