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
myApp.onPageInit('main-page', function(page) {
  mainPage.onPageInit(myApp, $$, mainView, page));
});
...
```

myapp/js/main-page.js
```javascript
var mainPage = {};

mainPage.onPageInit = function(myApp, $$, mainView, page) {
  myApp.alert('load complete!');
}


module.exports = mainPage;
```

위와같이 main-page가 onPageInit()됬을때 수행부를 별도의 파일로 떼어낼수 있다.

## ECMA Script 2015로 코딩하기
myapp$ npm install babel-core babel-loader babel-preset-es2015 --save-dev
ES2015를 사용하기 위해 바벨관련 패키지를 설치한다.

myapp/webpack.config.js
```javascript
var path = require('path');
module.exports = {
    entry: __dirname + '/js/my-app.js',
    output: {
        path: __dirname + '/www/js',
        filename: 'my-app.js'
    },
    module: {
      loaders: [
        {
          test: path.join(__dirname, 'js'),
          loader: 'babel-loader',
          query: {
              presets: ['es2015']
          }
        }
      ]
    },
    devtool: '#inline-source-map'
}
```
위와같은 webpack 설정파일을 만들면 앞으로는 파라메터 없이
myapp$ webpack --watch
로만 실행하면 된다.
이제 webpack이 output파일을 내놓기전에 babel-loader를 통해 먼저 ES2015를 브라우저가 해석할수 있는 구버전 문법으로 트랜스파일해준다.
```javascript
//var mainPage = require('./mainPage');
import mainPage from './mainPage.js';
```
모듈 파일을 포함할때도 require 대신 ES2015의 문법(import)를 사용할 수 있으며,
prototype 상속 대신 class 상속도 사용할수 있게 된다.
(자세한 ES2015문법은 https://babeljs.io/docs/learn-es2015 에서 참조)

## ESLint로 문법 체크하기
$ npm install eslint -g

myapp$ eslint --init
위 명령어를 실행하면 eslint 설정파일을 생성하기 위한 몇가지 질문을 한다.
아래는 내취향에 따라 선택한 값들이다.
1. User a popular style guide
2. AirBnB
3. JSON

다 끝나면 myapp/.eslintrc.json 파일이 생성된것을 볼수 있다. (마지막에 선택한 타입에 따라 확장자가 달라질수 있다)
이제 webpack 에서 작업을 수행할때 eslint로 문법도 체크하도록 하자

npm install eslint-loader --save-dev

myapp/webpack.config.js
```javascript
var path = require('path');
module.exports = {
    entry: __dirname + '/js/my-app.js',
    output: {
        path: __dirname + '/www/js',
        filename: 'my-app.js'
    },
    module: {
      // <<<<<<<<<< 추가된 부분 시작
      preLoaders: [
        {
          test: path.join(__dirname, 'js'),
          loader: 'eslint-loader'
        }
      ],
      // 추가된부분 끝 >>>>>>>>>>
      loaders: [
        {
          test: path.join(__dirname, 'js'),
          loader: 'babel-loader',
          query: {
              presets: ['es2015']
          }
        }
      ]
    },
    // <<<<<<<<<< 추가된 부분 시작
    eslint: {
      configFile: '.eslintrc.json'
    },
    // 추가된부분 끝 >>>>>>>>>>
    devtool: '#inline-source-map'
}
```
preLoaders 부분과 eslint 부분, 두부분이 추가되었다.
이제 myapp$ webpack 을 실행하면 ES2015에 위배되는 문법 오류나 경고를 보여줄것이다.

## PhoneGap(Cordova) Plugin 추가
기본 플러그인은 https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-battery-status/index.html 문서보고 기본 사용법을 익히면 된다.
https://cordova.apache.org/plugins
https://build.phonegap.com/plugins
http://plugins.telerik.com/cordova
위의 주소에서 각 기관/업체에서 검증된 플러그인을 검색할 수도 있다.

먼저 아래 명령어중 하나로 플러그인을 설치한다.
phonegap plugin add [plugin name]
cordova plugin add [plugin name]

index.html 파일 끝부분에 cordova.js를 추가해준다.
```
...
    <script type="text/javascript" src="cordova.js"></script>
    <!-- Path to Framework7 Library JS-->
    <script type="text/javascript" src="js/framework7.min.js"></script>
    <!-- Path to your app js-->
    <script type="text/javascript" src="js/my-app.js"></script>
  </body>
</html>
```
my-app.js 파일에 deviceready 콜백을 지정해주면 앱로드가 완료되고 코르도바 플러그인이 통신할 수 있을때 onDeviceReady()가 호출된다.
```
...
window.onload = function(){
  document.addEventListener("deviceready", onDeviceReady, false);
}

function onDeviceReady() {
  // 플러그인 초기화를 여기서 수행한다.
}
...
```

## Daum Map API 사용
http://developers.daum.net 에서 앱을 만든 후 API키를 발급받는다. 이때 하이브리드앱은 "모든 플렛폼"으로 발급 받아야 한다.
또한 생성한 앱설정에 보면 "쿼터변경" 버튼이 있는데, 여기에서 사용할 서비스를 TEST에서 SERVICE로 변경해줘야한다.
이후 index.html 마지막 부분에서
```
    <script type="text/javascript" src="http://apis.daum.net/maps/maps3.js?apikey=발급받은API키"></script>
    <script type="text/javascript" src="cordova.js"></script>
    <!-- Path to Framework7 Library JS-->
    <script type="text/javascript" src="js/framework7.min.js"></script>
    ...
```
위와같이 스크립트를 추가하고(src가 //로 시작하는데, http://로 변경해줘야만 한다)
지도를 표시할 페이지에서
```
                      <div id="map" style="width:500px;height:400px;"></div>                    
```
지도 크기를 지정한 태그를 적어준다.
마지막으로 my-app.js에서 해당 페이지에서 지도 설정값을 셋팅해주면 된다.
예를들어 위의 div태그가 map.html에 있다면 (data-page도 map으로 되어있음)
```
myApp.onPageInit('map', function(page) {
  var container = document.getElementById('map');
  var options = {
    center: new daum.maps.LatLng(33.450701, 126.570667),
    level: 3
  };
});
```
위와같이 onPageInit()때 map에 지도 셋팅을 해주면 하이브리드 앱에서도 다음지도를 볼 수가 있다.

## CrossWalk 사용하기

안드로이드 4.4 버전부터는 기본 웹뷰가 크로미움으로 바뀌어서 별 문제 없는데,
이전 버전은 웹뷰 퍼포먼스가 떨어져 애니메이션 같은걸 구동시키면 끊기는 느낌을 심하게 받는다.
이땐 아래의 명령어로 CrossWalk를 플러그인으로 설치해주면 부드럽게 하이브리드앱을 구동할 수 있다.
다만 CrossWalk도 내부적으론 Chromium을 사용하므로, 빌드 패키지가 그만큼 용량이 증가한다. (20MB정도)
```
$ cordova plugin add cordova-plugin-crosswalk-webview
$ cordova build --release
```
설치후 꼭! cordova clean 명령어을 한번 수행해주자. 그래야 run이나 build 명령어로 apk를 새로 뽑았을때 crosswalk가 제대로 적용된다.

## iOS에선 UIWebView 대신 WkWebView를 사용하기

iOS에선 안드로이드와 달리 폰갭용 CrossWalk 플러그인이 지원되지 않는다. UIWebView로도 충분한 성능이 나오지만 좀 더 개선된 WkWebView로 변경해보자.

cordova ios platform version 3 : https://github.com/Telerik-Verified-Plugins/WKWebView
cordova ios platform version 4 : https://github.com/apache/cordova-plugin-wkwebview-engine

버전 3와, 버전4로 나뉘는데 최신인 버전4가 좋겠지만, file:// 프로토콜에서 CORS이슈 때문에 F7에서의 페이지 이동이 안된다.
```
cordova platform list
```
위 명령어로 iOS 플렛폼의 버전을 확인해본 후, 3보다 크다면
```
cordova platform remove ios
cordova platform add ios@3
```
위 명령어로 3.x.x의 iOS 플렛폼 버전을 추가해주도록 한다.
이후, https://github.com/Telerik-Verified-Plugins/WKWebView 의 설치법을 참고하여 설치한 뒤,
```
cordova build ios
```
명령어로 빌드한 후, platforms/ios/projectname.xcodeproj 를 열어서 device에 넣으려고 하면 bitcode 관련 링크 에러가 발생하게 된다.

![](http://csbok.github.io/mdwiki-seed/ll_CC/pages/uploads/images/bitcode_enable.png)

위 이미지를 참고하여, bitcode를 yes가 아닌, no 로 바꿔준 후 프로젝트 저장 -> 클린 -> 다시 빌드 해보면 디바이스에서 WkWebView가 정상적으로 실행되는것을 볼 수 있다.
단, WkWebView로 바꾼경우 ajax 통신이 안될수 있으니, 서버에서 12344 포트(플러그인 설치시 바꿀 수 있음)에 대해 CORS를 설정해주도록 한다. 
(사용해봤지만 여러가지 문제가 많아 프로덕션에선 제거하였다)

## Release APK 생성하기
```
cordova build android --release
```
명령을 수행하면 android-armv7-release-unsigned.apk 파일이 생성된다. 
이 파일에 키사인을 적용하고, zipalign을 적용시키면 폰에 직접 다운로드 받을 수도 있고, 플레이마켓에도 배포할 수 있게 된다.
```
// keystore 파일 생성
$ keytool -genkey -v -keystore [파일명].keystore -alias [알리아스] -keyalg RSA -keysize 2048 -validity 10000

// keystore 를 사용하여 apk sign
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore [파일명].keystore android-armv7-release-unsigned.apk [알리아스]

// sign이 잘 되었는지 verify
$ jarsigner -verify -verbose -certs android-armv7-release-unsigned.apk

// zipalign 적용
$ zipalign -v 4 android-armv7-release-unsigned.apk [출력파일명].apk
```
