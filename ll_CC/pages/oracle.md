# Oracle

http://www.oracle.com/technetwork/database/database-technologies/express-edition
개발할때는 무료인 XE 버전을 사용

# 접속 클라이언트

- SQL Developer
http://www.oracle.com/technetwork/developer-tools/sql-developer/
오라클 사이트에서 제공하는 무료 접속 프로그램

- SQLGate
http://www.antwiz.com/kr/download/

- Orange
http://warevalley.com/xml/download/orange_trial

- Golden
http://www.benthicsoftware.com/golden.html

- Navicat
http://www.navicat.com

# 배우기 좋은 곳 
https://thebook.io/006696/
http://www.gurubee.net

# Ubuntu에서 설치
http://blog.saltfactory.net/linux/install-oracle-xe-on-ubuntu.html
위 블로그를 참조하여  리눅스용 rpm을 우분투에서 설치할 수 있다.

혹은 docker를 사용하여 쉽게 설치를 할 수 있다.
최근에 설치한 도커 이미지는 https://hub.docker.com/r/wnameless/oracle-xe-11g/ 이것이다.
README에 있는대로 도커를 실행시킨 후 docker exec -it &lt;name&gt; bash 명령어로 접속한 후
# sqlplus  /nolog
SQL>  connect  sys/oracle  as sysdba
