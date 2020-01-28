* Dockerfile

```dockerfile
FROM confluentinc/cp-kafka-connect:5.3.1

RUN   confluent-hub install --no-prompt debezium/debezium-connector-mongodb:0.10.0 \
   && confluent-hub install --no-prompt debezium/debezium-connector-mysql:0.10.0 \
   && confluent-hub install --no-prompt confluentinc/kafka-connect-hdfs:5.3.1 \
   && confluent-hub install --no-prompt mongodb/kafka-connect-mongodb:1.0.0
```

* Create image

```cmd
docker login example.com
docker build -t example.com/project/cp-kafka-connect:0.0.1 .
docker push example.com/project/cp-kafka-connect:0.0.1
```

* Create kube secret (run only once)

```cmd
kubectl create secret docker-registry example.com --docker-server=example.com --docker-username=user --docker-password=pw
```

* Modify values

```yaml
cp-kafka-connect:
  enabled: true
  image: example.com/project/cp-kafka-connect
  imageTag: 0.0.1
```

* Upgrade helm charts

```
helm status my-confluent
helm upgrade -f values.yaml my-confluent .
```

* Reference
  * <https://docs.confluent.io/current/connect/managing/extending.html>
  * <https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/>
  * <https://kubernetes.io/ko/docs/concepts/containers/images/>
  * API : https://docs.confluent.io/current/connect/references/restapi.html
