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
ionic start [app name] --ts tabs
```
위 명령을 실행하면 탭을 갖고있는 기본 템플릿이 생성된다.
--ts를 붙였기에 TypeScript 프로그래밍 환경이 만들어지며, --ts옵션을 안붙이면 자바스크립트로 생성된다.
하지만 AngularJS 2 공식 문서도 TypeScript 위주고, Ionic Framework 2의 문서 역시 타입스크립트로 안내되므로 --ts를 붙이는게 좋다.

phonegap/cordova를 래핑하였기에 똑같이 ionic server 명령어로 PC에서 확인하며 개발이 가능하다.
또한 ionic upload 명령어로 iOS, Android에 있는 ionic view앱에서 직접 실행도 가능하다.


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
