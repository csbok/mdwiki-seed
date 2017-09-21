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

# Springfox로 Swagger 적용하기

build.gradle 에 아래와 같은 디펜던시를 추가해준다.

```
    // swagger
	compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.7.0'
	compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.7.0'
```

이 내용을 SwaggerConfig.java 파일을 만들어 추가한다.

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2)
				.select().apis(RequestHandlerSelectors.any()) // 현재 RequestMapping으로 할당된 모든 URL 리스트를 추출.
				.paths(PathSelectors.ant("/api/**")) // 그중 /api/** 인 URL들만 필터링
	                		.build();
        }
}
```

출처 : http://jojoldu.tistory.com/31


```java
@RequestMapping(value = MyController.RESOURCE_PATH, consumes = MediaType.APPLICATION_JSON_VALUE, produces = MediaType.APPLICATION_JSON_VALUE)
public class MyController {
  
  @PreAuthorize(value = "#oauth2.clientHasRole('ROLE_APP') and hasRole('ROLE_USER')")
  @ApiOperation(value = "상세보기", notes = "상세보기 note")
  @ApiImplicitParam(name = "Authorization", value = "Authorization", required = true, dataType = "string", paramType = "header", defaultValue = "")
  public ResponseEntity<?> getDetail(@PathVariable("myCode") String myCode) {
    return ResponseEntity.ok(myService.getMyData(myCode));
  }
````

그리고 위와 컨트롤러에 consumes 항목을 지정하면 SwaggerUI에서 직접 REST를 호출할 수가 없다. (MediaType이 달라서)
해결법을 아직 못찾아서 consumes = MediaType.APPLICATION_JSON_VALUE 항목을 빼는 해결책 밖에는 모르겠다.

그리고 @PreAuthorize를 사용한 경우에는 REST호출 할때 Authorization을 넘겨야 하는데,
이땐 @ApiImplicitParam으로 필요한 값을 넣을 수 있게 해주면 된다. (swagger ui에서 넣을 수 있는 인풋폼이 나타나게 된다.)
만약 값이 2개 이상이라면 아래와 같이 @ApiImplicitParams()으로 감싸줘야 한다.

```java
  @ApiImplicitParams({
      @ApiImplicitParam(name = "Authorization", value = "Authorization", required = true, dataType = "string", paramType = "header", defaultValue = "")
      @ApiImplicitParam(name = "SecondValue", value = "SecondValue", required = true, dataType = "string", paramType = "query", defaultValue = "")
  })  
````

출처1 : http://yookeun.github.io/java/2017/02/26/java-swagger/
출처2 : https://ahea.wordpress.com/2017/01/18/spring-boot-swagger-적용기/ 

마지막으로 파라메터를 DTO로 전달받거나 할 경우, 각 멤버에 대한 설명은 아래와같이 @ApiModelProperty를 통해 설정을 한다.

```java
public class CostomerDto {

  // ssn은 swagger ui에서 보이지 않게 숨긴다.
  @ApiModelProperty(hidden = true)
  private String ssn;

  @ApiModelProperty(value = "고객명")
  private String name;
```

출처 : http://aoruqjfu.fun25.co.kr/index.php/post/528
