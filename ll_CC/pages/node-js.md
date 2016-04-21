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
