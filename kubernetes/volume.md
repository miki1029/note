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
```
```yaml
spec:
  volumes:
  - name: {volumeName}
    emptyDir:
      medium: Memory # tmpfs
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
```
```yaml
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

```yaml
spec:
  volumes:
  - name: {volumeName}
    configMap:
      name: {configMapName}
      items: # 개별 항목만 마운트할 경우에 명시
      - key: {configMapKey}
        path: {filename}
```

### PersistentVolume, PersistentVolumeClaim

* persistentVolumeClaim : 사전 또는 동적으로 프로비저닝된 영구 스토리지
* accessModes
  * ReadWriteOnce (RWO) : 단일 노드만 읽기/쓰기
  * ReadOnlyMany (ROX) : 여러 노드가 읽기
  * ReadWriteMany (RWX) : 여러 노드가 읽기/쓰기
* persistentVolumeReclaimPolicy
  * Retain : 수동 / 다시 사용하려면 PV 리소스를 삭제하고 다시 작성해야 함
  * Recycle ; 자동 / 볼륨의 내용을 삭제하고 볼륨을 다시 할당할 수 있게 함
  * Delete : 자동 / 기본 스토리지를 삭제
* PV는 클러스터 범위이므로 특정 네임스페이스에서 생성 불가
* PVC는 특정 네임스페이스에서 생성 가능


#### 수동 프로비저닝

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
  persistentVolumeReclaimPolicy: Retain # 중간에 변경 가능
  gcePersistentDisk:
    pdName: mongodb
    fsType: ext4

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc
spec:
  resources:
    requests:
      storage: 1Gi
  accessModes:
  - ReadWriteOnce
  storageClassName: "" # 사전 배포된 PV를 사용하려면 명시적으로 "" 설정

apiVersion: v1
kind: Pod
metadata:
  name: mongodb
spec:
  containers:
  - image: mongo
    name: mongodb
    volumeMounts:
    - name: mongodb-data
      mountPath: /data/db
    ports:
    - containerPort: 27017
      protocol: TCP
  volumes:
  - name: mongodb-data
    persistentVolumeClaim:
      claimName: mongodb-pvc
```

#### 자동 프로비저닝 (StorageClass)

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/gce-pd # PV 프로비저닝에 사용할 볼륨 플러그인
parameters: # 플러그인 매개변수
  type: pd-ssd
  zone: europe-west1-b

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc
spec:
  storageClassName: fast
  resources:
    requests:
      storage: 100Mi
  accessModes:
  - ReadWriteOnce
```

* 자동으로 PV가 생성된다.
  * 클레임 정책은 Delete

##### 기본 스토리지 클래스

```yaml
$ k get sc standard -o yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    storageclass.kubernetes.io/is-default-class: "true" # sc 기본값 설정
  creationTimestamp: "2019-12-17T13:27:59Z"
  labels:
    addonmanager.kubernetes.io/mode: EnsureExists
  name: standard
  resourceVersion: "403"
  selfLink: /apis/storage.k8s.io/v1/storageclasses/standard
  uid: a51fc6e6-5918-49ef-98df-47444068bf37
provisioner: k8s.io/minikube-hostpath # PV 프로비저닝에 사용할 볼륨 플러그인
reclaimPolicy: Delete
volumeBindingMode: Immediate

# 스토리지 클래스를 지정하지 않고 PVC 생성
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc2
spec:
  resources:
    requests:
      storage: 100Mi
  accessModes:
  - ReadWriteOnce
```
