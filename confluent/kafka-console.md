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
