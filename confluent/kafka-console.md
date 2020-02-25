```
# 컨슈머
$ kafka-console-consumer --bootstrap-server <broker1,broker2> --topic <topicName> --property print.key=true

# 컨슈머 그룹 offset reset
$ kafka-consumer-groups --bootstrap-server <broker1,broker2> --topic <topicName> --group <groupName> --reset-offsets --to-earliest --execute
```

* producer key : <http://bigdatums.net/2017/05/21/send-key-value-messages-kafka-console-producer/>
