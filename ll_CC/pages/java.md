## 서버 셋팅
개인적으로 우분투를 사용하여, 우분투 기준으로 정리한다.

sudo apt-get update

실행환경만 필요하면 jre를, 개발환경이 필요하면 jdk를 설치하도록 하자
ubuntu의 기본 jdk환경은 openjdk이다.

sudo apt-get install default-jre
sudo apt-get install default-jdk

직접 버전을 명시하고 싶은 경우는 아래의 패키지 명을 이용하면 된다.

sudo apt-get install openjdk-7-jdk

다음은 톰캣 설치이다.
sudo apt-get install tomcat7
설치가 완료되면 바로 ServerIP:8080 으로 접속하여 잘 동작하는지 확인할 수 있다.

/var/lib/tomcat7/webapps/ROOT
이제 개발을 한 후, war 파일을 위의 경로로 카피해준다.

sudo service tomcat7 restart
위 명령어로 톰캣을 재시작을 하면 war 파일과 같은 디렉토리가 생성되어 서비스 되는것을 볼 수 있다.

만약 문제가 발생한다면 /var/lib/tomcat7/logs 내에 있는 로그 파일들을 보자 (권한이 없으니 sudo su등으로 봐야한다)
권한이 없어서 war파일을 풀지 못 하여 직접 풀고 싶을때는 단순히 unzip으로 풀어주면 된다.
sudo unzip ProjectName.war -d ProjectName 


참고 : https://blog.lael.be/post/858


## 스프링 프레임워크에서 자주 사용되는 annotation
http://noritersand.tistory.com/156

Spring MVC에서 Controller가 뷰 파일을 지정하지 않고, 직접 스트링을 리턴하고 싶을땐 
@ResponseBody 를 사용하면 된다. 

@RequestMapping(value="/controller", method=GET)
@ResponseBody
public String foo() {
    return "Response!";
}
