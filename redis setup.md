---
references : 
- https://lucas-owner.tistory.com/56
- https://velog.io/@coastby/redis-docker%EB%A1%9C-redis-%EB%9D%84%EC%9A%B0%EA%B8%B0
---

# 1. Image Pull

redis에서 official docker image를 제공한다.

```bash
# latest version
> docker image pull redis

# 지정 version
> docker image pull redis:6.0.18
```

# 2. Docker Network 

redis를 사용하는 방법에는 여러가지가 있지만
redis를 편하게 사용하게 해주는 redis-cli를 사용하기 위해서는
redis-cli 컨테이너를 실행하고 redis, redis-cli 두 개의 컨테이너를 연결시켜줘야 한다.
(docker 컨테이너 내부 접속 후 redis-cli를 사용한다면 필요 없다.)

```bash
# network 생성
> docker network create redis-network
# network 확인
> docker network ls
```


# Docker Run

redis 서버를 실행한다.

```bash
> docker run \
-d \
--name redis \
-p 6379:6379 \
--network redis-network \
-e TZ=Asia/Seoul \
-v D:/wsl/instances/docker-desktop/redis/redis.conf:/etc/redis/redis.conf \
-v D:/wsl/instances/docker-desktop/redis/data:/data \
redis:latest redis-server /etc/redis/redis.conf
```


# Redis 접속 방법

### 1) 별도 컨테이너로 redis-cli 실행
현재 실행 중인 Redis Server 외에 또 하나의 컨테이너(redis_cli)를 실행하고 네트워크를 통해 접속
```bash
> docker run -it --network redis-network --rm redis:latest redis-cli -h redis
```

### 2) redis server 컨테이너 내부에 접속하여 redis-cli 실행
redis server 컨테이너 내부에 접속하여, redis-cli 를 실행하여 접속

```bash
> docker exec -it myredis redis-cli --raw
```
※ --raw 옵션은 redis-cli 를 사용할때 한글을 정상적으로 출력하기 위해 사용하는 옵션