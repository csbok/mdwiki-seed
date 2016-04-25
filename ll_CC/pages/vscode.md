# Visual Studio Code

https://www.visualstudio.com/ko-kr/products/code-vs.aspx

MS에서 오픈소스로 개발하고 있는 에디터. Atom이나 Bracket처럼 웹기술 기반으로 만들어져서 여러 운영체제에서 잘 돌아간다.

# Intellisense를 위해 tsd 설치하기 
$ npm install -g tsd

위 명령어로 tsd 를 먼저 설치한후, 프로젝트 디렉토리로 가서 필요한 타입정의를 설치한다.
예를들어 express의 인텔리센스를 보고싶다면 아래와 같이 입력한다.

$ tsd install express --save

어떠한 타입정의들이 지원되는지 알고싶으면 아래 사이트에서 검색해보면된다.
http://definitelytyped.org/tsd/

# Debug

VS Code는 기본으로 node.js의 디버깅이 가능하다.
node.js 프로젝트 폴더를 열은 후, 
![](https://code.visualstudio.com/images/debugging_debugicon.png)
위의 디버그 버튼을 누르면 좌측 트리뷰가 디버그 뷰로 바뀌는데, Play버튼을 누르면 launch.json 파일 템플릿을 생성해준다.
본인의 프로젝트에 맞게(program의 항목을 시작파일, 예를들어 app.js 혹은 index.js등으로 변경) 설정한뒤 
Node.js 환경을 선택하면 해당 js파일을 debug모드로 실행시켜준다. (다른 언어 확장도 깔면 그 언어 디버깅도 가능하다)
다른 IDE처럼 브레이크 포인트 걸고 사용하면된다.
더 자세한 내용은 공식홈페이지를 참고하자.
https://code.visualstudio.com/Docs/editor/debugging
