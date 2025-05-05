# Redis Practice

## 기본
- redis는 0 ~ 15번까지의 database로 구성
- select 명령어 사용하여 이동
```bash
> select db번호
```

- 현재 database 내 모든 key 조회
```bash
> keys *
```

- 현재 database 내 모든 key 삭제
```bash
> flushdb
```

## String 구조

### SET
- set을 통해 key, value 등록
- 추가 옵션 사용 가능

**syntax**
```bash
> SET key value [NX | XX] [GET] [EX seconds | PX milliseconds | 
  EXAT unix-time-seconds | PXAT unix-time-milliseconds | KEEPTTL]
```

**options**

a) NX / XX
- NX : key가 존재하지 않는 경우만 set (not exists의 약자)
- XX : key가 존재하는 경우만 set

b) GET
- GET 옵션은 기존에 저장된 key의 값을 반환하면서 새로운 값을 설정하는 기능
- set으로 값을 변경하는 동시에 그 이전 값을 받아볼 수 있음 

c) TTL(time to live)
- EX seconds : 지정한 시간(초 단위) 이후로 만료 설정
- PX milliseconds : 지정한 시간(밀리초 단위) 이후로 만료 시간 설정
- EXAT timestamp-seconds : 지정한 유닉스 시간(초 단위)으로 만료 시간 설정 (Expire At 약자)
- PXAT timestamp-milliseconds : 지정한 유닉스 시간(밀리초 단위)으로 만료 시간 설정 (PExpire At 약자)
- KEEPTTL : 대상 key의 기존 만료 시간을 유지

**examples**

```bash
> set user:email:1 user1@gmail.com
> set user:email:2 user2@gmail.com nx
> set user:email:3 user3@gmail.com ex 10

# redis 활용 - 사용자 인증 정보 저장 (refresh token)
> set user:1:refresh_token eyjal12z9f0alda ex 100000
```

### GET
- get을 통해 key가 가진 value를 조회

**syntax**
```bash
> GET key
```

**examples**
```bash
> get user:email:1
```

### DEL
- del을 통해 하나 또는 여러 개의 key를 삭제

**syntax**
```bash
> DEL key1 key2 key3 ...
```

**examples**
```bash
> DEL mykey
> DEL key1 key2 key3
```
