# Atom Editor

GitHub에서 만든 에디터.
공식사이트 : https://atom.io/
[Brackets](http://www.brackets.io), [Visual Studio Code](https://www.visualstudio.com/products/code-vs)와 마찬가지로 웹기술로 개발되었다.
덕분에 OS를 가리지 않고 여러 환경에서 구동된다.
참고 : http://blog.gaerae.com/2015/05/sublimetext-brackets-atom-visualstudiocode.html

## 기본적인 사용법
OpenTutorials에 잘 정리되어있다.
https://opentutorials.org/module/1579

## 개인적으로 유용한 package
https://atom.io/packages/emmet
https://atom.io/packages/atom-beautify
https://atom.io/packages/minimap
https://atom.io/packages/split-diff
https://atom.io/packages/activate-power-mode

https://atom.io/packages/atom-html-preview
brackets 처럼 수정하고 있는 파일을 지속적으로 리프래시 하여 보여준다.
npm의 liver-server와도 같은 역할을 한다.

https://atom.io/packages/atom-typescript
https://atom.io/packages/autoclose-html

# atom-ide

먼저 아톰을 설치한다.
https://atom.io 

현재 ide-java 는 atom의 베타 버전만을 지원하므로, ide-java를 쓸 생각이라면
https://atom.io/beta 에서 베타버전을 설치한다. (하지만 ide-java가 제대로 동작하지 않는것 같다.)

https://ide.atom.io
ide 설명은 위의 사이트에서 볼 수 있고, 아톰 에디터를 띄운 후 setting(preferences) > install에서 
atom-ide-ui를 설치한다.

그 후, 자신이 사용할 랭귀지에 대한 플러그인을 설치해야한다.
ide-typescript 은 깔면 바로 typescript, javascript를 바로 지원해준다.

현재 버전에서 ide-java는 제대로 동작하지 않는것 같다.

그리고 파이썬은 ide-python을 깐 후에
pip install python-language-server 를 설치해줘야 한다.

virtualenv 환경에 깔면 동작 안하는것 같다. 어쩔수 없이 글로벌 환경에 설치하였다.
