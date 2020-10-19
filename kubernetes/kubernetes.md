* livenes probe
  * HTTP GET probe : check 2xx or 3xx
  * TCP socket probe : check TCP port
  * Exec probe : check 0

```yml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-liveness
spec:
  containers:
  - image: luksa/kubia-unhealthy
    name: kubia
    livenessProbe:
      httpGet:
        path: /
        port: 8080
```
