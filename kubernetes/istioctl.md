# istioctl

```
# display available profiles
istioctl profile list

# display proflie configuration (실제 프로파일의 설정파일 내용보다 많은 내용이 출력됩니다.)
istioctl profile dump demo
istioctl profile dump demo --config-path components.pilot

# show difference
istioctl profile diff default demo

# 현재 설정을 dump
kubectl -n istio-system get IstioOperator installed-state -o yaml > installed-state.yaml
```

```
# install using default profile
istioctl install

# install using predefined profile
istioctl install --set profile=demo -y

# install using default profile with overriding properties (유지보수성이 떨어지기 때문에 권장하지 않음)
istioctl install --set meshConfig.accessLogFile=/dev/stdout

# 설정 파일로 install
istioctl install -f my-config.yaml

# external helm chart
istioctl install --manifests=manifests/
```
