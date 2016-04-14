## Node.JS 설치
https://nodejs.org 를 방문하여 Node.JS를 설치한다.

## npm으로 폰갭 설치
$ npm install phonegap -g

## 폰갭 프로젝트를 생성
$ phonegap create [DirName] [Identifier] [ProjectName]
ex) phonegap create myapp net.finesoft.myapp MyApp

## Framework7 설치
http://framework7.io 에서 직접 zip을 받거나 bower로 설치
zip파일 내부에 dist 폴더만 사용한다.

위에서 만든 폰갭 프로젝트 디렉토리(이하 myapp$) 으로 이동하여 기존 www 폴더를 삭제한다.
dist폴더를 myapp폴더로 이동 후 www로 이름을 바꾼다.

myapp$ phonegap serve
위 명령어로 서버를 구동 한 후 브라우저나 디바이스의 phonegap app에서 미리보기를 할 수 있다.
여기까지만 해도 Framework7을 이용한 하이브리드앱 개발을 할 수 있지만,
JavaScript은 모듈화가 안되서 my-app.js가 금방 덩치가 커지고 지저분해질 수 있다.
아래부터는 이를 해결하기 위한 WebPack 사용법이다.

## WebPack 설치
$ npm install webpack -g

## WebPack으로 모듈화
myapp$ mkdir js
myapp$ cp ./www/js/my-app.js ./js/
myapp$ webpack --watch ./js/my-app.js ./www/js/my-app.js
위 명령어를 실행하면, 이후부터는 myapp/www/js/my-app.js 파일을 직접 수정하는 것이 아니라,
myapp/js/my-app.js 파일을 수정하면 webpack이 컴파일(모듈을 합침)하여 myapp/www/js/my-app.js로 출력을 해준다.

myapp/js/my-app.js
```javascript
...
var mainPage = require('./main-page');
myApp.onPageInit('main-page', mainPage.onPageInit(myApp, $$, page));
...
```

myapp/js/main-page.js
```javascript
var mainPage = {};

mainPage.onPageInit = function(myApp, $$, page) {
  myApp.alert('load complete!');
}

module.exports = mainPage;

```

위와같이 main-page가 onPageInit()됬을때 수행부를 별도의 파일로 떼어낼수 있다.
