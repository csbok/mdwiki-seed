# 자주 쓰는 명령어
df -h
남은 용량 확인하기

du -hs .
현재 디렉토리 용량 확인하기

ps -aux
현재 실행중인 프로세스 목록 전체 보기

# SSH 설치
``` bash
dpkg -l | grep ssh
```
명령어로 openssh-server 가 설치되어 있는지 확인한다.

``` bash
sudo apt-get install openssh-server
```
없으면 위 명령어로 설치를 해준 후
``` bash
netstat -ntl
```
위 명령어로 22 번 포트에서 잘 작동 되는지 확인한다.
