# Volume

<https://kubernetes.io/docs/concepts/storage/volumes>

## 볼륨 유형

### 일시적 스토리지

#### emptyDir

* 일시적인 데이터
* 동일한 pod에서 실행 중인 컨테이너 간에 파일을 공유할 때
* 컨테이너의 파일 시스템에 쓰기가 가능하지 않을 때
* pod를 호스팅하는 워커 노드의 실제 디스크에 생성됨

```yaml
spec:
  volumes:
  - name: {volumeName}
    emptyDir: {}

spec:
  volumes:
  - name: {volumeName}
    emptyDir:
      medium: Memory
```

#### gitRepo (deprecated)

* 깃 스토리지의 내용을 체크아웃
* 정적 페이지를 git repo에 저장하여 사용하려고 할 때
* 생성된 후에는 repo와 동기화되지 않음
* 도커 허브의 git sync로 대체?

```yaml
spec:
  volumes:
  - name: {volumeName}
    gitRepo:
      repository: {gitUrl}
      revision: master
      directory: . # default는 git repo name이 된다.
```

### 노드 스토리지

#### hostPath

* 워커 노드의 파일 시스템에서 포드의 디렉터리를 마운트
* 노드의 로그 파일, kubeconfig, CA 인증서 등에 주로 사용
* pod의 데이터를 저장할 목적으로 사용하면 안됨 : pod이 다른 노드로 이동하게 되면 사용할 수 없게 됨

### 영구 스토리지

* NAS(Network-Attached Storage) 유형

#### nfs

```yaml
spec:
  volumes:
  - name: {volumeName}
    nfs:
      server: {ip}
      path: {path}
  containers:
  - volumeMounts:
    - name: {volumeName}
      mountPath: {mountPath}
```

#### 클라우드 제공자

* gcePersistentDiisk, awsElastic-BlockStore, azureDist, ...

```yaml
spec:
  volumes:
  - name: {volumeName}
    gcePersistentDisk:
      pdName: {gce pd name}
      fsType: ext4
  containers:
  - volumeMounts:
    - name: {volumeName}
      mountPath: {mountPath}

spec:
  volumes:
  - name: {volumeName}
    awsElasticBlockStore:
      volumeId: {EBS volume id}
      fsType: ext4
  containers:
  - volumeMounts:
    - name: {volumeName}
      mountPath: {mountPath}
```

#### 기타

* cinder, cephfs, iscsi, flocker, glusterfs, quobyte, rbd, flexVolume, vsphereVolume, photonPersistentDisk, scaleIO
  * 다른 유형의 네트워크 스토리지
* configMap, secret, downwardAPI : 특정 쿠버네티스 리소스 및 클러스터 정보를 포드에 노출하는 데 사용되는 특수한 유형의 볼륨 (쿠버네티스의 메타 데이터를 실행)

### PersistentVolume, PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongodb-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnece
  - ReadOnlyMany
  persistentVolumeReclaimPolicy: Retain
  gcePersistentDisk:
    pdName: mongodb
    fsType: ext4
```

* persistentVolumeClaim : 사전 또는 동적으로 프로비저닝된 영구 스토리지
