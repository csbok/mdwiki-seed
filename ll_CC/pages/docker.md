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
sudo docker run -i -t --name my-ubuntu uhbuntu:14.04 /bin/bash
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

# 명령행 실행하는 방법
docker exec -it 컨테이너이름 /bin/bash


# 변경 사항을 새로운 도커 이미지로 생성
my-ubuntu 컨테이너를 만들어서 서버 운영에 필요한 프로그램들을 설치하고, 배포 코드까지 같이 올리면 할일은 끝난다.
다만, 이 작업들이 컨테이너에 반영됬을뿐이지 이미지는 변함없이 그대로이다. (hdd에 프로그램깔았다고 윈도우 설치 CD가 바뀌지는 않는것 처럼)
이제 컨테이너의 변동사항을 새로운 이미지로 구워보자.
```
sudo docker diff my-ubuntu
```
버전 관리 시스템의 diff 처럼 어떤 파일이 추가됬고, 수정됬는지 목록을 보여준다.
```
sudo docker commit my-ubuntu-work
```
위와 같이 입력하면, 변동 사항을 포함한 새로운 나만의 이미지를 만들어준다. 새로 생성된 이미지는 sudo docker images로 확인할 수 있다.

# 도커 이미지 파일로 또 다른 서버에 배포
이렇게 만든 my-ubuntu-work 이미지를 일관된 개발환경을 위해 사용하거나, 클라우드 환경에서 새로운 인스턴스(서버)를 추가하여 도커로 배포 환경을 꾸릴려면 도커 이미지를 파일로 추출해야한다.
참고로, hub.docker.com 을 이용해서 push, pull을 받는 방법도 있지만, 추후에 알아보겠다.
```
sudo docker save -o 경로 이미지이름
ex) sudo docker save -o ./my-ubuntu-work-file my-ubuntu-work
```
위 명령어를 사용하면 해당 경로에 도커 이미지 파일이 생성이 된다. 이 파일을 복사하여 개발환경이나, 새로 운영할 서버에 도커를 설치한 후, 아래의 명령어를 실행해주면 기존의 my-ubuntu-work를 똑같이 사용할 수 있다.
```
sudo docker load -i 경로
ex) sudo docker load -i ./my-ubuntu-work-file
```

# 그외 유용한 또다른 옵션들
docker run 을 할때 -i 대신 -d 옵션을 주면 백그라운드 데몬으로 실행할 수 있으며 -p host포트:container포트 옵션으로 포트 포워딩을 할수 있다. 예를 들어 도커 컨터이너에 웹서버를 설치하여 80포트로 서비스를 제공할때, -p 8888:80 을 주면 http://호스트IP:8888 으로 접근할 경우 도커 컨테이너의 80으로 접근하게 된다.

# 사용하지 않는 컨테이너와 이미지 삭제
더이상 쓸모 없는 컨테이너가 있을시에는
```
sudo docker rm my-ubuntu
```
위의 명령어로 컨테이너를 제거할 수 있으며, 이미지의 경우 
```
sudo docker rmi my-ubuntu-work
```
위의 명령어로 이미지를 제거할 수 있다. 이미지를 제거하기 전에 그 이미지를 사용하는 컨테이너를 먼저 삭제하자.


이외에 Dockerfile을 기술하여 컨테이너를 적재할때 일괄적으로 작업절차를 수행시킬수도 있다. 자세한것은 참고페이지들에서 익히자.

## 참고페이지
http://pyrasis.com/Docker/Docker-HOWTO
http://www.slideshare.net/pyrasis/docker-fordummies-44424016
https://opentutorials.org/course/128/8657
http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/
