# Python

공식사이트는 http://www.python.org

데이터 분석같은 분야에서 사용할 경우에는 여러 패키지가 미리 설치되어 있는 아나콘다를 설치하는것이 편하다.
https://www.continuum.io/downloads

아나콘다로 설치를 했으면 jupyter도 사용할 수있다.
$ jupyter notebook
위 명령어를 실행하면 웹 인터페이스로 되어있는 명령어 입력 쉘을 실행할 수 있다.
웹브라우저에 jupyter notebook이 실행되면 우측의 new 버튼을 눌러 Python을 선택하면 된다. 

## jupyter notebook 에서 그래프 그리는 방법
%matplotlib inline
import matplotlib.pyplot as plt
plt.plot(range(20),range(20))

위와 같이 %matplotlib inline 를 먼저 입력하고 matplotlib를 사용하면, plt.show()를 호출하지 않아도 jupyter notebook 내부에 그래프가 보인다. 
