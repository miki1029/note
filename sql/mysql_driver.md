# mysql driver의 serverTimezone의 역할

jpa에서 OffsetDateTime을 사용하면 이는 mysql driver 단에서 java.sql.Timestamp로 변환되어 사용됩니다.
즉, 현재 serverTimezone에 따라서 다르게 저장되는 부분은 jpa와 관계가 없으며 java.sql.Timestamp를 파라미터로 사용하는 PreparedStatement를 사용시 영향이 있습니다.

java.sql.Timestamp를 파라미터 바인딩하는 시점에 serverTimezone에 맞춰서 `yyyy-MM-dd HH:mm:ss` 포맷으로 변환하게 됩니다.
이 과정에서 serverTimezone과 실제 mysql server의 timezone이 다르게 되면 unix timestamp가 손실될 수 있는 문제가 발생하게 됩니다.
따라서 serverTimezone 파라미터는 가급적 사용하지 않는 방향이 맞아 보입니다.
아래는 mysql driver 코드의 일부입니다.

```
// ClientPreparedQueryBindings.java
public void setTimestamp(int parameterIndex, Timestamp x, Calendar targetCalendar, int fractionalLength) {
	// ...

	this.tsdf = TimeUtil.getSimpleDateFormat(this.tsdf, "''yyyy-MM-dd HH:mm:ss", targetCalendar,
	                    targetCalendar != null ? null : this.session.getServerSession().getDefaultTimeZone());

	StringBuffer buf = new StringBuffer();
	buf.append(this.tsdf.format(x));

	// ...
}
```

결론
* serverTimezone 파라미터는 사용하지 않는다.
* mysql server는 각 국가의 timezone으로 실행한다.(사실 이 부분은 UTC로 실행해도 큰 문제는 없습니다.)
* app server는 어떤 타임존으로 띄우더라도 mysql로 저장시에는 mysql server의 timezone으로 변환하여 저장하기 때문에 아무런 문제가 없다.
