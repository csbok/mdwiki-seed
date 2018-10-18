# React 16 버전으로 IE9 지원하기

## Create React App을 사용하는 경우

CRA v2를 사용하는 경우는 [이 글](https://velog.io/@velopert/create-react-app-v2)을 참조하여 react-app-polyfill을 추가로 설치하고 src/index.js파일의 가장 첫줄에 아래의 문장을 써주면 된다.

```javascript
import 'react-app-polyfill/ie9'; // For IE 9-11 support
```



mobx를 같이 쓰는 경우는 mobx5는 지원을 못하니 mobx4에서 가장 높은 4.5.0을 설치해주자.



## 사이트에 CDN으로 삽입하는 경우

기본적으로 리액트문서를 보면 개발시에는 아래와같이 스크립트태그를 사용하게 되어있다.

```html
<!-- Note: when deploying, replace "development.js" with "production.min.js". -->
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.devel2opment.js" crossorigin></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script type="text/babel" src="./index.js"></script>
```

추가로 위 부분보다 더 상단에 아래의 스크립트를 포함시켜주면 사이트에 CDN으로 삽입해도 잘 돌아간다.

```html
<!-- requestAnimationFrame과 cancelAnimationFrame을 위한 폴리필 -->
<script src="https://cdn.jsdelivr.net/npm/request-animation-frame-polyfill@0.1.3/index.js"></script>
<!-- react의 set과 reactDom 관련 폴리필 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.0.0/polyfill.js"></script>
```



# Create React App 에서 multiple entry point 적용하기 (MPA에서 React 사용하기)

https://github.com/facebook/create-react-app/issues/1084#issuecomment-430822863

