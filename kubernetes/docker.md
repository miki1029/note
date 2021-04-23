# Docker

## Commands

### Getting Started

```bash
➜  docker git:(master) ✗ git clone https://github.com/docker/doodle.git
Cloning into 'doodle'...
remote: Enumerating objects: 73, done.
remote: Total 73 (delta 0), reused 0 (delta 0), pack-reused 73
Unpacking objects: 100% (73/73), done.

➜  docker git:(master) ✗ cd doodle/cheers2019 && docker build -t miki1029/cheers2019 .
Sending build context to Docker daemon   12.8kB
Step 1/9 : FROM golang:1.11-alpine AS builder
1.11-alpine: Pulling from library/golang
9d48c3bd43c5: Pull complete
7f94eaf8af20: Pull complete
9fe9984849c1: Pull complete
ec448270508e: Pull complete
65ba82af53f7: Pull complete
Digest: sha256:09e47edb668c2cac8c0bbce113f2f72c97b1555d70546dff569c8b9b27fcebd3
Status: Downloaded newer image for golang:1.11-alpine
 ---> e116d2efa2ab
Step 2/9 : RUN apk add --no-cache git
 ---> Running in 6aa0593ba608
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
(1/5) Installing nghttp2-libs (1.39.2-r0)
(2/5) Installing libcurl (7.66.0-r0)
(3/5) Installing expat (2.2.8-r0)
(4/5) Installing pcre2 (10.33-r0)
(5/5) Installing git (2.22.2-r0)
Executing busybox-1.30.1-r2.trigger
OK: 21 MiB in 20 packages
Removing intermediate container 6aa0593ba608
 ---> e77cc05f0f76
Step 3/9 : RUN go get github.com/pdevine/go-asciisprite
 ---> Running in 8affe828653a
Removing intermediate container 8affe828653a
 ---> 588d977e74f4
Step 4/9 : WORKDIR /project
 ---> Running in b096f91b6f0e
Removing intermediate container b096f91b6f0e
 ---> 156e7a54fc73
Step 5/9 : COPY cheers.go .
 ---> 104517647b35
Step 6/9 : RUN CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cheers cheers.go
 ---> Running in 296e983a406f
Removing intermediate container 296e983a406f
 ---> 0d1c77f86011
Step 7/9 : FROM scratch
 --->
Step 8/9 : COPY --from=builder /project/cheers /cheers
 ---> c0259d28c70c
Step 9/9 : ENTRYPOINT ["/cheers"]
 ---> Running in 645159470d51
Removing intermediate container 645159470d51
 ---> f343a1eb2225
Successfully built f343a1eb2225
Successfully tagged miki1029/cheers2019:latest

➜  cheers2019 git:(master) docker run -it --rm miki1029/cheers2019

➜  cheers2019 git:(master) docker login && docker push miki1029/cheers2019
Authenticating with existing credentials...
Login Succeeded
The push refers to repository [docker.io/miki1029/cheers2019]
7110e23f387e: Pushed
latest: digest: sha256:7c91396470ab03d8a2b0de4a83656b58da02aee215fb7a0c0117ac7902dfeb56 size: 528
```

### Commands

<https://docs.docker.com/engine/reference/commandline/cli/>

```bash
# 정보 출력
docker images
docker ps
docker ps -a # 중지된 컨테이너까지 모두 출력
docker ps -a -q # id만 출력
docker inspect <container-name>
docker logs <container-name>
docker history <container-name>

# build
docker build -t <image>[:<tag:latest>] .
docker build -t kubia:1.0 .

# run
docker run <image>[:<tag>] [<command>]
docker run busybox echo "hello world"

docker run --name <conainer-name> -p <local-port>:<container-port> -d <image> --rm
docker run --name <conainer-name> -p <local-port>:<container-port> -d <image> --rm -v <local-path>:<container-path> # volume mount

# exec
docker exec -it <container-name> bash # i: STDIN / t: pseudo terminal(TTY)

# stop
docker stop <container-name or id>

# rm
docker rm <container-name>
docker rmi <image-name>

# tag
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

# repo
docker search <image-name>
docker pull <image-name>
docker inspect <image-name> # 포트 정보 등 확인 가능
docker push [OPTIONS] NAME[:TAG]

# cp
docker cp <path> <container>:<path>
docker cp <container>:<path> <path>
docker cp <container>:<path> <container>:<path>
```

* 이미지가 로컬에 없으면 도커 허브에서 최신 이미지를 다운 받는다.
* 컨테이너를 생성한다.
* 명령어를 실행한다.
* 컨테이너가 중지된다.

### Dockerfile

<https://docs.docker.com/engine/reference/builder/>

```Dockerfile
FROM <image>:<tag>
ADD <local-file> <container-file>
ENTRYPOINT [<command>, <arg1>, <arg2>, ...]
```

```Dockerfile
FROM node:7
ADD app.js /app.js
ENTRYPOINT ["node", "app.js"]
```
