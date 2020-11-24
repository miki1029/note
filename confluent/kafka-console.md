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

to-earliest 부분에 대신 사용할 수 있는 옵션들

```
--shift-by <Long: number-of-offsets> 형식 (+/- 모두 가능)
--to-offset <Long: offset>
--to-current
--by-duration <String: duration> : 형식 ‘PnDTnHnMnS’
--to-datetime <String: datetime> : 형식 ‘YYYY-MM-DDTHH:mm:SS.sss’
--to-latest
--to-earliest
```

```
TOPIC=my-topic
GROUP=my-consumer

# 특정 시각으로 오프셋 리셋 (dry-run : 실제 오프셋을 적용하지 않고 적용될 결과를 보여주기만 함)
kafka-consumer-groups\
    --bootstrap-server $BOOTSTRAP_SERVER\
    --topic $TOPIC\
    --group $GROUP\
    --reset-offsets --to-datetime 2020-11-24T10:30:00.000+09\
    --dry-run

# 특정 시각으로 오프셋 리셋 (execute : 실제 오프셋을 적용)
kafka-consumer-groups\
    --bootstrap-server $BOOTSTRAP_SERVER\
    --topic $TOPIC\
    --group $GROUP\
    --reset-offsets --to-datetime 2020-11-24T10:30:00.000+09\
    --execute
    
# 오프셋을 처음으로 리셋 (execute : 실제 오프셋을 적용)
kafka-consumer-groups\
    --bootstrap-server $BOOTSTRAP_SERVER\
    --topic $TOPIC\
    --group $GROUP\
    --reset-offsets --to-earliest\
    --execute

# 메시지 확인
kafka-avro-console-consumer\
    --bootstrap-server $BOOTSTRAP_SERVER\
    --property schema.registry.url=$SCHEMA_REGISTRY_URL\
    --property print.key=true\
    --topic $TOPIC\
    --group $GROUP

# 컨슈머 그룹 삭제
kafka-consumer-groups --bootstrap-server $BOOTSTRAP_SERVER --delete --group $GROUP
```

* <https://docs.confluent.io/5.5.2/quickstart/ce-quickstart.html>
