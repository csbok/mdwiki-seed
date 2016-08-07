http://pyrasis.com/Docker/Docker-HOWTO
http://www.slideshare.net/pyrasis/docker-fordummies-44424016
https://opentutorials.org/course/128/8657
http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/

# 설치
아래와 같은 명령어를 실행하면 리눅스 배포판을 신경쓸 필요 없이 알아서 설치가 된다.
```
curl -s https://get.docker.com | sudo sh
```

설치 후 docker 명령어를 사용할땐 sudo가 항상 필요하며, 이게 번거로울 시에는 docker 그룹에 현재 유저를 포함하는 방법이 있다.
단, 루트 권한을 갖게 되므로 주의할것. 방법은 참고 URL 참조.
```
sudo service docker start
```
위의 명령어로 시작을 하거나 start 대신 restart 로 재시작을 할 수 있다

# 도커 이미지 검색
도커에는 이미지와 컨테이너라는 개념이 있는데, 이미지는 CD라고 생각하면 편하고, 컨테이너는 실제 하드에 설치되서 실행되는 환경을 생각하면 편한다.
```
sudo docker search ubuntu
```
위의 명령어로 받아서 설치할 수 있는 이미지를 검색해볼 수 있으며, https://hub.docker.com 를 통하여 웹에서도 볼수가 있다.
username/ubuntu 는 다른 유저들이 만든 이미지고, ubuntu처럼 제품 이름만 있는 이미지들이 공식 이미지이다.

# 도커 이미지 받기
git 명령어와 유사하게 사용할 수 있다.
```
sudo docker pull ubuntu:14.04
```
: 뒤에 특정 버전을 받을수 있다. ubuntu:latest로 최신버전을 받을 수도 있다.
받은 이미지를 확인해보고 싶으면 아래의 명령어를 수행한다.
```
sudo docker images
```

# 도커 컨테이너 적재
```
sudo docker run -i -t --name my-utubntu uhbuntu:14.04 /bin/bash
```
위 명령어를 수행하면 이미지를 컨테이너로 적재한다. CD로 운영체제를 하드에 까는것과 같은 작업이라고 생각하면 된다.
--name 뒤에는 원하는 이름을 준다. 생략하여도 무관하지만, 추후 다른 명령어에서 쉽게 사용하기 위해 지정해주는 것이 좋다.
적재가 완료 되면 /#의 쉘이 나타나고 그 환경은 도커가 깔린 호스트의 환경과는 격리되있는 독립 환경이 된다.
이제 이 독립 환경에 내가 필요한 프로그램들과 환경을 설정해주면 된다.
예를들어 git을 설치하고 싶으면 아래와 같은 명령을 입력하면 된다. (/# 은 도커 컨테이너에서 실행된 쉘을 의미)
```
/# apt-get update
/# apt-get install git
```
이제 도커 컨테이너에서 실행된 쉘에서 빠져 나오려면 exit를 입력하거나, 
Ctrl + P, Ctrl + Q를 순차적으로 입력하여 쉘을 일시정지 시키면 된다.
exit 를 입력하여 쉘을 빠져나올 경우 아예 컨테이너가 종료되기때문에, 서버 프로그램이 돌고 있다면 단축키로 빠져나와야 한다.

컨테이너의 목록을 보고 싶으면 아래와 같은 명령어를 수행한다.
```
sudo docker ps -a
```
-a  를 생략하면 현재 구동중인 컨테이너만 볼 수 있고, -a 를 붙이면 중단된 컨테이너를 포함해서 전체를 볼 수 있다.

