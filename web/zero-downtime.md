# Zero Downtime

* keep-alive를 사용하는 경우 zero-downtime 구현을 위해서 서버측, 클라이언트측 모두 신경 써주어야 하는 부분이 있습니다.

## Server-side

* LB에서 제거(쿠버네티스의 경우 Service에서 Pod를 제거)된 이후 실제 shutdown 동작을 하기 전까지 애플리케이션은 다음과 같이 동작해야 합니다.

1. 신규 커넥션을 허용하지 않아야 합니다.
2. 기존 keep-alive 커넥션에서 요청이 들어올 경우 응답 헤더에 `Connection: close` 를 포함하여 더이상 keep-alive를 허용하지 않음을 클라이언트에게 알려야 합니다.
3. 10초정도 sleep 합니다.
4. 이후 남은 요청 또는 작업이 있을 경우 마저 처리가 될 수 있게 graceful shutdown을 수행합니다.

## Client-side

1. server에서 idle keep-alive connection을 끊음과 동시에 클라이언트가 해당 커넥션으로 request를 전송하면 오류가 발생합니다. 클라이언트는 이 오류를 재시도할 수 있어야 합니다.
    * 프록시를 사용하지 않는 경우 또는 프록시와 클라이언트 간의 커넥션이 끊어진 경우 : `java.net.SocketException: Unexpected end of file from server`
    * istio-proxy를 사용하고 서버와 프록시간의 커넥션이 끊어진 경우 : `[503 Service Unavailable] ...(생략) : [upstream connect error or disconnect/reset before headers. reset reason: connection termination]`

2. 서버가 종료 중일때 10초정도 sleep을 한다면 이보다 짧은 텀(5초)으로 keep-alive timeout을 설정해야 합니다.
    * 1번이 잘 설정됐다면 2번의 요구사항을 어느정도 대체할 수 있으므로 필수적인 설정은 아닙니다.
