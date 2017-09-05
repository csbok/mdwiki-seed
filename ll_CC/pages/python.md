
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

## Web Programming
micro framework 가  필요할 경우에는 Flask,
full stack framework 의 경우는 django를 선택한다.
django에서 rest api를 구현할땐 django rest framework를 선택하자.

django 튜토리얼은 아래의 문서가 잘 정리되어 있다.
https://tutorial.djangogirls.org/ko/

몇가지 빠진걸 추가하자면, mac이나 linux에선 venv를 실행하기 위해 source ./mysite/bin/activate와같이 명령어를 내려야한다.
production의 경우 웹서버에 연결후 서비스를 하는게 맞지만, 테스트를 위해 서버에서 manage.py로 구동하는 경우는 아래와 같이 구동해야 접근 할 있다.
python manage.py runserver 0.0.0.0:8000

또한 도메인  기반으로 접근할땐,
settings.py파일에 다음과 같이 변경한다.
ALLOWED_HOSTS = ['ionic.finesoft.net', 'localhost', '127.0.0.1']

## Django Rest Framework

CORS 이슈를 만난다면 다음의 패키지를 설치해서 해결할 수 있다.

https://github.com/ottoyiu/django-cors-headers/

### AWS S3에 이미지 올리기
```
pip install boto3
pip install django-storages  # s를 꼭 붙여야 한다.
```
위의 패키지들을 설치해주고 settings.py에 다음과 같은 값을 셋팅한다.

DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
AWS_ACCESS_KEY_ID = 'AWS액세스키'
AWS_SECRET_ACCESS_KEY = 'AWS시크릿액세스키'
AWS_STORAGE_BUCKET_NAME = 'S3버킷이름'

단, 서울이나 프랑크푸르트와 같이 최근에 생성된 리전의 경우 Signature Version 4만 지원하기 때문에 동작하지 않을 수 있다.
이때는 아래와 같이 리전을 강제로 한국(ap-northeast-2)로 지정해주면 된다. [ref](http://sebatyler.github.io/2016/07/16/django-storages-seoul.html)
AWS_S3_REGION_NAME = 'ap-northeast-2'

또 다른 방법으론 
AWS_QUERYSTRING_AUTH = False
플래그를 셋팅하여, 이미지 url뒤에 붙는 get method의 키=값들을 사용하지 않는 것이다.
단 이 방식을 사용하게 되면, S3에서도 권한 체크를 하지 않게 아래와 같이 권한 설정을 해줘야 한다. [ref](http://djangotricks.blogspot.kr/2013/12/how-to-store-your-media-files-in-amazon.html)
버킷선택 후 Permissions탭에서 Bucket Policy에 아래의 값을 넣는다.
```
{
    "Version": "2017-07-17",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::내버킷이름/*"
        }
    ]
}
```
단 이방식의 경우, 모두에게 읽기 권한을 열어주게 되므로 url만 안다면 expired 없이 누구든 언제나 접근가능하게 되어 S3의 GET접근 COUNT가 예상보다 많이 오르게 되어 비용이 더 나올수 있으므로, 이 방식보다는 위의 방식(리전을 직접 지정하는 방식)을 추천한다.

## Crawling
단순 문서 파싱에는 Beautiful Soup을 쓰는게 좋고,
주기적으로 여러 범위에서 크롤링을 해올땐 프레임워크 형태로 제공되는 Scrapy가 좋다.

아래는 Scrapy에 대한 설명이다.
```
pip install scrapy
```
윈도우의 경우에는 Microsoft Visual C++ Build Tools (http://landinghub.visualstudio.com/visual-cpp-build-tools)가 있어야 설치가 된다.
(없을경우 에러메시지에서 설명해줌)

```
scrapy shell '크롤링할 url'
```
위의 명령어로 REPL쉘을 열어서 크롤링할 사이트에 대해서 탐색해볼 수 있다.
윈도우의 경우 실행할때 ModuleNotFoundError: No module named 'win32api' 에러가 났는데
```
pip install pypiwin32
```
설치해주면 해결된다.
쉘에서
```
>>> response.xpath('//title/text()').extract()
```
위의 코드로 title의 내용을 추출해 낼 수 있다.
