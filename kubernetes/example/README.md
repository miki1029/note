* <https://github.com/luksa/kubernetes-in-action>

# Yaml

* List PV : <https://github.com/luksa/kubernetes-in-action/blob/master/Chapter10/persistent-volumes-gcepd.yaml>
* headless Service : <https://github.com/luksa/kubernetes-in-action/blob/master/Chapter10/kubia-service-headless.yaml>
* StatefulSet : <https://github.com/luksa/kubernetes-in-action/blob/master/Chapter10/kubia-statefulset.yaml>

# Examples

* API 서버를 통해 Pod과 통신하기

```
{apiServerHost}:{port}/api/v1/namespaces/{namespace}/pods/{pod}/proxy/{path}
{apiServerHost}:{port}/api/v1/namespaces/{namespace}/services/{service}/proxy/{path}
```
