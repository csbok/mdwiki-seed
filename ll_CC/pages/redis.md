# redis
## 설치
```bash
$ sudo apt-get update
$ sudo apt-get install redis-server
```

## 설정
외부에서 아무나 붙게 하려면 /etc/redis/redis.conf 파일에서 bind 127.0.0.1 를 bind 0.0.0.0 로 바꿔준 후 aws에서 6379 포트를 풀어주면 된다.  
외부에서 telet redis-server-ip 6379 로 확인해보자

## 시작, 중단, 재시작
```bash
$ /etc/init.d/redis-server start
$ /etc/init.d/redis-server stop
$ /etc/init.d/redis-server restart
```
