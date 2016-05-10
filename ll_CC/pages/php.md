# PHP

Modern PHP로 오면서 많은것이 바뀌고 있다.
[Composer](https://getcomposer.org)를 사용한 의존성 관리도 가능해졌다.

PHP7으로 개발을 시작할땐 http://modernpug.github.io/php-the-right-way 를 먼저 읽어보자.

## Heroku에 배포할때

Heroku에서 PHP + CodeIgniter 2 + MySQL 환경을 구동하려고 하는데
DB에만 접근하면 아무런 에러메시지 없이 500 에러를 냈다.

검색해보니 로컬에서 컴포저를 설치하고 composer.json 파일에 아래와 같은 내용을 넣어준다.

{
"require": {
    "ext-mysql": "*"
   }
}

그후 composer install 명령을 실행하면 composer.lock과 vender 폴더가 생성되는데 전부 heroku에 올려주면 제대로 동작한다.

composer install 명령을 실행할땐 에러가 났는데
다음과 같은 두 경우였다.

1. open ssl이 사용안함으로 되있어서 (따로 설치안해도 되는것 같다)
php.ini에서 extension=php_openssl.dll (리눅스는 .so확장자)만 주석(;) 제거해주면된다.

2. mysql 설정이 안되있어서 (역시 따로 설치 안해도 되는것 같다)
php.ini에서 extension=mysql.dll (리눅스는 .so확장자)만 주석(;) 제거해주면된다.


## 개발환경
윈도우즈라면 [WAMP Server](http://wampserver.com/en)를 사용하여 쉽게 Apache + PHP + MySQL 환경을 구축할 수 있다.
XDebug가 포함된 패키지를 설치하여 디버그도 쉽게 가능하다.
VSCode를 이용한다면
1. [PHP Debug Plugin](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)을 설치하고 재시작 한다.
2. php.ini 파일에서
```
[XDebug]
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
```
위와 같은 설정을 추가한 후 apache를 재시작 한다.
3. VSCode에서 톱니바퀴 모양 아이콘(디버그 버튼)을 눌러 launch.json을 자동생성 한 후 디버깅 모드로 진입한다.




