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

# nginx를 node.js의 reverse proxy로 설정하기
``` bash
sudo apt-get install nginx
sudo service nginx start
```
위 명령어로 nginx를 설치하고 실행시켜준다.

``` bash
sudo vi /etc/nginx/sites-available/node
```
그리고 사이트를 위한 설정 파일(위 명령어에선 node)을 만들어서 아래와 같은 내용을 입력해줍니다.

```
server {
        listen 80;
        server_name my.domain.com;
        location / {
                proxy_pass http://127.0.0.1:4000;
        }
}
```
proxy_pass에는 node.js로 실행되는 서비스의 포트번호를 붙여주시고,
server_name에는 접근될 도메인 주소를 넣습니다. (물론 CNAME을 포함한 도메인 설정은 도메인 관리 페이지를 통해서 해야합니다.)
ip를 사용하실 경우는 server_name 행을 지우시고, listen 80 변경해주세요. (default 와 겹치기 때문입니다. 80을 사용하시려면 default를 지우시거나 /etc/nginx/site-available/default 를 직접 수정하세요)

``` bash
sudo ln -s /etc/nginx/sites-available/node /etc/nginx/sites-enabled/node
sudo service nginx reload
```
마지막으로 sites-enable에 site-available에서 방금 만든 node의 심볼릭 링크를 걸어준 후 서비스를 재시작하면 됩니다.
이젠 nginx를 통해 서비스를 제공하므로 node.js의 서비스 포트는 방화벽애서 막는것이 좋습니다. (위의 예에서는 4000번 포트)

# nginx에 php 연동 시키기 (CodeIgniter 사용시)
```
sudo apt-get install php5-fpm;
sudo apt-get install php5-mysql
sudo apt-get install php5-mcrypt
sudo php5enmod mcrypt
```
nginx에서 php를 구동하기 위해선 php5-fpm이 필요하다.

```
sudo vi /etc/nginx/sites-enabled/default
```
그리고 nginx의 설정을 위해 /etc/nginx/sites-enabled/default 파일을 수정해주자.
php 만 사용할땐 [이 문서](http://www.tutorialbook.co.kr/entry/Ubuntu-에서-NginX-PHPFPM-연동하기)를 보고 수정하면 되지만,
code igniter를 사용할땐 [이 문서](http://codeigniter-kr.org/bbs/view/tip?idx=8326)를 보고 수정하는것이 좋다.

설정이 끝났으면 php5-fpm과 nginx를 재시작해주면 된다.
```
sudo service php5-fpm restart
sudo service nginx restart
```
