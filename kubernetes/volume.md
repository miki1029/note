# Volume

## 볼륨 유형

* emptyDir : 일시적인 데이터
* hostPath : 워커 노드의 파일 시스템에서 포드의 디렉터리를 마운트
* gitRepo : 깃 스토리지의 내용을 체크아웃
* nfs : NFS 공유
* gcePersistentDiisk, awsElastic-BlockStore, azureDist, ... : 클라우드 제공자 전용
* cinder, cephfs, iscsi, flocker, glusterfs, quobyte, rbd, flexVolume, vsphereVolume, photonPersistentDisk, scaleIO
  * 다른 유형의 네트워크 스토리지
* configMap, secret, downwardAPI : 특정 쿠버네티스 리소스 및 클러스터 정보를 포드에 노출하는 데 사용되는 특수한 유형의 볼륨 (쿠버네티스의 메타 데이터를 실행)
* persistentVolumeClaim : 사전 또는 동적으로 프로비저닝된 영구 스토리지
