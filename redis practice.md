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

## 숫자 다루기 (정수)
- 캐싱된 데이터를 redis에서 바로 연산을 수행해서 저장할 수 있음
- 키가 존재하지 않으면 연산을 수행하기 전에 0으로 설정 됨
- 키가 잘못된 유형의 값을 포함하거나 정수로 표현할 수 없는 문자열 값을 포함하고 있으면 오류가 발생 함

### INCR / DECR
- 값을 1씩 증가/감소시킬 수 있음

**syntax**
```bash
> INCR key
> DECR key
```

**examples**
```bash
> incr likes:posting:1
> decr likes:posting:1
```

### INCRBY / DECRBY
- 값을 지정한 숫자만큼 증가/감소시킬 수 있음

**syntax**
```bash
> INCRBY key increment
> DECRBY key increment
```

**examples**
```bash
> incrby likes:posting:1 10
> decrby likes:posting:1 10
```


## List 구조
- redis의 list는 deque 자료구조

### LPUSH / RPUSH
- 데이터를 좌/우 끝에 삽입
- 복수 개의 데이터를 나열하면 연달아 저장할 수 있음 

**syntax**
```bash
> LPUSH key value [value ...]
> RPUSH key value [value ...]
```

**examples**
```bash
> LPUSH fruits apple banana kiwi
> RPUSH numbers 1 2 3 4 5
```

### LPOP / RPOP
- 데이터를 좌/우 끝에서 꺼냄
- pop된 데이터는 key에서 사라짐 

**syntax**
```bash
> LPOP key
> RPOP key
```

**examples**
```bash
> LPOP fruits
> RPOP numbers
```

### LRANGE
- 입력한 인덱스 범위의 요소를 반환

**syntax**
```bash
> LRANGE key start stop
```
- start 0, stop 0 : 첫번째 요소만 반환
- start 0, stop -1 : 전체 요소를 반환
- start -1, stop -1 : 마지막 요소만 반환
- start -2, stop -1 : 마지막 두번째부터 마지막 요소까지 반환
- start 0, stop 1 : 첫번째부터 두번째 요소까지 반환

**examples**
```bash
> LRANGE fruits 1 3
1) "apple"
2) "banana"
3) "kiwi"

> LRANGE numbers 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
```

### LLEN
- 리스트에 존재하는 데이터의 개수 조회

**syntax**
```bash
> LLEN key
```

**examples**
```bash
> LLEN fruits
3
```

### TTL 적용 / 조회
- 저장된 key에 TTL 적용할 수 있음

**syntax**
```bash
# TTL 적용
> EXPIRE key seconds
# TTL 조회
> TTL key
```

**examples**
```bash
# TTL 적용
> EXPIRE fruits 5
# TTL 조회
> TTL fruits
```


## Set 자료 구조
- 데이터를 중복, 순서 없이 보관하는 구조
