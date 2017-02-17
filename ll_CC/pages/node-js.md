# node.js

https://www.nodejs.org
https://www.npmjs.com

## 유의할점

node.js는 싱글쓰레드 방식이라 1번코어만 일하고 나머지 코어는 놀게 된다.
이걸 해결하기 위해 cluster라는 모듈을 쓰게 되는데 이 모듈이 하는 일은 해당 프로세스를 fork()해주는것이다.
프로세스 자체가 fork되다보니 메모리공간도 따로 처리하는 프로세스도 따로인지라, cluster 모듈을 붙였을때 여러가지 사이드이펙트가 나타난다.

1. node-inspector를 붙일때도 cluster를 통해 프로세스가 4개로 fork되면, 다른 프로세스가 처리를 해버려서 브레이크포인트에서 멈추지 않는 경우
2. socket.io 에서 1번 프로세스에 붙은 사람이 3번 프로세스 붙은사람과 통신 못하는 상황 (해결법 : https://blog.outsider.ne.kr/764)

위와 같은 상황을 주의해서 개발해야한다.

# Cluster Mode
예전에는 CPU를 여러개 사용하기 위해 직접 cluster 모듈을 사용하여 코드 내에 fork()코드를 입력해줘야했지만
forever 대신 pm2를 사용하게 되면 별도의 코드 없이 명령어만으로 클러스터 모드로 동작시킬 수 있다.
```pm2 start app.js -i 0```
위와 같이 입력하면 CPU갯수만큼 프로세스가 생성되게 된다. (0 대신 숫자를 직접 입력하면 그 숫자만큼 생성된다.)
더 자세한 내용은 https://blog.outsider.ne.kr/1197 를 읽어보자
 
# versioning
짝수 버전이 지원 기간이 긴 LTS 이다. (프로덕션에서 사용할만함)
홀수 버전은 Stable 버전으로 새로운 피쳐를 적용해나가는 버전이다.
신기술이 필요할땐 Stable버전을, 서버에 프로덕션으로 설치할땐 LTS가 좋다.

# npm
https://www.npmjs.com
위 주소에서 node.js의 package 들을 볼수 있다.
프로젝트 폴더에 package.json 파일이 있을경우
npm install 명령어로 의존성 있는 패키지들을 설치할 수 있다.
또한 npm install -g [PackageName] 으로 패키지를 전역으로 설치할 수 있다.
별도 명령어를 지원하는 경우 전역으로 설치하면 커맨드명령어로 쓸 수 있다.
또한 npm outupdated / npm outupdated -g 도 있는데
설치 된 패키지/전역패키지의 현재버전과 최신버전을 알 수 있다.
npm ls 명령어로 현재 프로젝트 모듈들의 설치 상황도 알 수 있다.
