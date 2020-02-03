# Helm

## 정보 출력

```
helm version
helm search repo <repo-name>
helm status <name>
helm ls
```

## repo 설정

```
# repo 추가 (공식 헬름 차트 - 디폴트로 제공되진 않음)
helm repo add <repo-name> <url>
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

# repo 업데이트
helm repo update

# 정보 출력
## Helm Hub 에서 검색
helm search hub [package-name]
## 내 repo에서 검색
helm search repo [package-name]
```

## 업데이트

```
helm repo update
```

## 패키지 관리

```
helm install <my-package-name> <repo-name>/<package-name> [-n=<namespace-name>]
helm uninstall <my-package-name> [-n=<namespace-name>]
```

```
helm upgrade -f <file.yaml> <name> .
```

## Reference

* <https://helm.sh/docs/intro/quickstart/>
* <https://hub.helm.sh/>
* <https://github.com/RehanSaeed/Helm-Cheat-Sheet>
