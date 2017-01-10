# Ionic Framework

하이브리드 앱 제작을 위한 프레임워크
공식 배포처 : http://ionicframework.com

Native 관련 부분은 Cordova를 래핑하여 사용하고 있고,
웹기술쪽은 Angular JS를 사용한다.

1.x버전은 Angular 1을 사용하며, Ionic2는 Angualr 2를 사용한다.

또한 http://ionic.io 에서는 Ionic을 플렛폼화 하여,
- 프로토타입을 위한 Creator (https://creator.ionic.io)
- 미리보기를 위한 Ionic View 모바일앱 (http://view.ionic.io)
- 빌드와 배포를 위한 Ionic Lab (http://lab.ionic.io)
환경을 지원해준다.

환경이나 API, 문서화등도 깔끔하게 잘 정리되있어 사용자 층이 두텁다.

## 시작하기
```
ionic start [app name] --v2 --ts tabs
```
위 명령을 실행하면 탭을 갖고있는 기본 템플릿이 생성된다.
--ts를 붙였기에 TypeScript 프로그래밍 환경이 만들어지며, --ts옵션을 안붙이면 자바스크립트로 생성된다.
하지만 AngularJS 2 공식 문서도 TypeScript 위주고, Ionic Framework 2의 문서 역시 타입스크립트로 안내되므로 --ts를 붙이는게 좋다.

phonegap/cordova를 래핑하였기에 똑같이 ionic server 명령어로 PC에서 확인하며 개발이 가능하다.
또한 ionic upload 명령어로 iOS, Android에 있는 ionic view앱에서 직접 실행도 가능하다.

## REST 통신하기
```
$ ionic platform add android
$ ionic platform add ios
$ cordova plugin add cordova-plugin-whitelist
```
먼저 iOS와 Android 플렛폼을 추가한 뒤, whitelist plugin을 설치한다. (플러그인은 플렛폼 단위로 설치되니 플렛폼 추가를 먼저하자)
### GET방식으로 통신하기
통신할 페이지로 가서 코드를 아래와 같이 변경한다.
```
import {Page} from 'ionic-angular';
import {Http} from '@angular/http';
import 'rxjs/add/operator/map';

@Page({
  templateUrl: 'build/pages/page3/page3.html'
})
export class Page3 {
  constructor(http: Http) {

    http.get('http://localhost:3000/').map(res => res.json()).subscribe(data => alert(data.message));
  }
}
```
Http를 DI로 받아, get명령어로 호출하면 {message:'test'}와 같은 json 출력을 해당 주소(localhost:3000)에서 받아올 수 있다.

### POST 방식으로 통신하기
```
import {Page} from 'ionic-angular';
import {Http, Headers, Response} from '@angular/http';
import 'rxjs/add/operator/map';

@Page({
  templateUrl: 'build/pages/page3/page3.html'
})
export class Page3 {
  constructor(http: Http) {
    var headers = new Headers();
    headers.append('Content-Type', 'application/json');
    this.http.post(
      'http://localhost:3000/login',
      JSON.stringify({email:this.user.username, password:this.user.password}),
      {headers:headers}
    ).map((res: Response) => res.json())
    .subscribe(res => { alert(res.message); });
  }
}
```
위의 코드로 작업할 경우 node.js + express + body-parser로 되있는 서버 환경에서 값을 무사히 받아온다.
서버측 코드는 다음과 같다.
```
var express = require('express');
var cors = require('cors');
var bodyParser = require('body-parser');
var app = express();

app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded());

app.get('/', function (req, res) {
  res.send({success: true, message: 'this is a test message'});
});

app.post('/login', function(req,res) {
  console.log('email : ' + req.body['email']);
  console.log('password : ' + req.body['password']);

  res.send({success:true, message:'succes!'});
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```
하지만 php + code-igniter 환경에서 form으로 전송받는 탓에 위의 값들이 제대로 넘어가지 않았다.
php + code-igniter 환경이라면 아래와 같이 header와 bodydata를 수정해줘야 한다.
```
...
var headers = new Headers();
headers.append('Content-Type', 'application/x-www-form-urlencoded'); // application/json 대신 사용
this.http.post('http://localhost/login',
'email='+this.user.username+'&password='+this.user.password, // JSON.stringify 대신 사용
{headers:headers})
...
```

## Daum Map API 사용
http://developers.daum.net 에서 앱을 만든 후 API키를 발급받는다. 이때 하이브리드앱은 "모든 플렛폼"으로 발급 받아야 한다.
또한 생성한 앱설정에 보면 "쿼터변경" 버튼이 있는데, 여기에서 사용할 서비스를 TEST에서 SERVICE로 변경해줘야한다.
이후 index.html 상단 meta 태그들이 모여있는 부분에
```
<meta http-equiv="Content-Security-Policy" content="default-src *; script-src 'self' 'unsafe-inline' 'unsafe-eval' *; style-src  'self' 'unsafe-inline' *">
```
를 추가하고, 파일의 긑 부분에서
```
    <script type="text/javascript" src="http://apis.daum.net/maps/maps3.js?apikey=발급받은API키"></script>
    <script type="text/javascript" src="cordova.js"></script>
    ...
```
위와같이 스크립트를 추가하고(src가 //로 시작하는데, http://로 변경해줘야만 한다)
지도를 표시할 페이지(ex: page1.html)에서
```
                      <div id="map" style="width:500px;height:400px;"></div>                    
```
지도 크기를 지정한 태그를 적어준다.
이후 ts파일(ex: page1.ts)의 코드를 다음과 같이 작성한다.
```
import {Page, Platform} from 'ionic-angular';

@Page({
  templateUrl: 'build/pages/page1/page1.html',
})
export class Page1 {
  platform: Platform;
  
  constructor(platform: Platform) {
    this.platform = platform;
    this.initMap();
  }
  
  initMap() {
    this.platform.ready().then(() => {
      var container = document.getElementById('map'); //지도를 담을 영역의 DOM 레퍼런스
      var options = { //지도를 생성할 때 필요한 기본 옵션
        center: new daum.maps.LatLng(33.450701, 126.570667), //지도의 중심좌표.
        level: 3 //지도의 레벨(확대, 축소 정도)
      };

      var map = new daum.maps.Map(container, options); //지도 생성 및 객체 리턴
    });
  }
}
```
platform.ready()는 cordova의 onDeviceReady()와 같은 역할이다. 디바이스의 준비가 완료되면,
다음맵 api 예제 코드를 그대로 붙여넣으면 지도가 정상 동작한다.
다음맵 api는 JavaScript코드지만, 어짜피 TypeScript가 JavaScript의 슈퍼셋이므로 별 무리 없이 돌아간다.

## 테마 변경
ionic framework 2는 구동되는 환경에 따라서 iOS, Material Design(Android), Windows Phone Theme 이렇게 3가지 스타일을 제공한다.
하지만 어느환경에서도 한가지 테마로 정해서 보이고 싶을때가 있는데 이때는 app/app.ts 파일을 열어서 다음을 수정한다
```
@App({
  
  ...
  
  config: {
    mode: 'md'
  }
})
export class MyApp {
...
```
| Platform      | Mode           |
| ------------- |----------------|
| iOS           | mode: 'ios'    |
| Android       | mode: 'md'     |
| Windows       | mode: 'wp'     |
위와 같은 값으로 config의 mode값을 변경하면 된다.

## Moment 사용하기
npm install moment --save

import moment from 'moment';
import 'moment/src/locale/ko';

## ion-img 에서 높이 100%로 적용되는 문제
```
<ion-img class="expanded-image" alt="Loading" [src]="imageUrl"></ion-img>

.expanded-image img {
  min-height: initial;
}
```
출처: https://github.com/driftyco/ionic/issues/7473

## label 태그에서 for 속성 사용할때
타입스크립트의 for 예약어때문에 attr.for 로 사용해줘야한다 
https://github.com/angular/angular/issues/5435

## CLI
ionic g page PAGENAME
ionic g provider PROVIDERNAME
ionic g component COMPONENTNAME

등의 명령어로 템플릿 적용된 파일들을 쉽게 생성할 있다.

## Input
&lt;calendar [year]="2017" [month]="1"&gt;&lt/calendar&gt;
컴포넌트를 만들때 엘리먼트에 프로퍼티를 전달 하고 싶은 경우, [ ] 괄호로 감싸서 넘겨준다.
```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'calendar',
  template: '<div (click)="clickHandler('click!')">{{year}} {{month}}</div>'
})
export class Calendar {
    @Input() year: number;
    @input() month: number;

    constructor() {
        // 이곳에선 @Input으로 넘어온 값을 읽을 수가 없다.
    }

    ngOnInit() {
        // ngOnInit에서 읽어야 한다.
        console.log(year + ' / ' + month);
    }
}
```
받는 컴포넌트 측에서는 생성자에선 값을 못 받아오고, ngOnInit() 시점 이후에 읽을 수 있다.
만약 ngOnInit에서도 @Input값이 설정이 안되면(undefined라던가) 로그를 찍어봐서, ngOnInit이 2번 호출되는건 아닌지 체크할 필요가 있다.
나의 경우 angular2 프로젝트에서 컴포넌트를 @NgModule의 declarations에만 넣어주면 되는데, bootstrap안에 까지 넣어서 2번 호출되는 상황이 발생하였다.
[https://github.com/angular/angular/issues/6782](https://github.com/angular/angular/issues/6782)에서 amardeep157 님의 글에서 해결책을 찾았다.

참고 : [http://learnangular2.com/inputs](http://learnangular2.com/inputs/)  
