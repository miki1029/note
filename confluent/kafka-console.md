## topic

```
$ kafka-topics\
  --create\
  --topic <topicName>\
  --replication-factor <number>\
  --partitions <number>\
  --bootstrap-server <broker1,broker2>
```

## producer

```
$ kafka-console-producer\
  --topic <topicName>\
  --broker-list <broker1,broker2>\
  --property "parse.key=true"\
  --property "key.separator=:"
```

## consumer

```
$ kafka-console-consumer\
  --bootstrap-server <broker1,broker2>\
  --topic <topicName>\
  --from--beginning
  --property print.key=true

# 컨슈머 그룹 offset reset
$ kafka-consumer-groups\
  --bootstrap-server <broker1,broker2>\
  --topic <topicName>\
  --group <groupName>\
  --reset-offsets\
  --to-earliest\
  --execute
```

to-ealiest 부분에 대신 사용할 수 있는 옵션들

```
--shift-by <Long: number-of-offsets> 형식 (+/- 모두 가능)
--to-offset <Long: offset>
--to-current
--by-duration <String: duration> : 형식 ‘PnDTnHnMnS’
--to-datetime <String: datetime> : 형식 ‘YYYY-MM-DDTHH:mm:SS.sss’
--to-latest
--to-earliest
```
