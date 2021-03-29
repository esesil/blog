---
title: "[인프런]스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 01"
date: 2021-03-29 월요일 22:00:00+0900
excerpt: "스프링부트 프로젝트 환경설정"
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
last_modified_at: 2021-03-29T08:06:00-05:00
---

> 인프런, 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 강의를 듣고 정리한 내용입니다.

## 프로젝트 생성

- 자바 8을 자바 11로 업데이트하는 것부터 시작했다. 환경변수 잘 설정해줬는데도 `javac -version` 는 자바11인데 `java -version`은 자바8로 나왔다. 버전변경이 제대로 안 되었던 이유는 path에 다른 요소가 설정되어 있어서 그랬다ㅜㅜ 아래 두개가 path에 있으면 삭제해주자

  - C:\ProgramData\Oracle\Java\javapath
  - C:\Program Files (x86)\Common Files\Oracle\Java\javapath

- 스프링부트 스타터 사이트로 이동해 스프링 프로젝트 생성
  - https://start.spring.io/
  - Maven vs Gradle : 필요 라이브러리를 가져오고 빌드하는 라이프사이클 까지 관리해줌. 최근엔 Gradle을 씀
  - 스프링부트 버전
    - snapshot : 아직 만들고 있는 버전
    - M1 : 정식 릴리즈 안 된 버전
    - 아무것도 안 써져있는 걸 쓰자!
  - Project Metadata
    - Group : 보통 기업명, 도메인명 작성
    - Artifact : 빌드 된 결과물(프로젝트명)
  - Dependencies : 어떤 라이브러리를 가져와서 쓸 것인가?
    - spring web : web project 할거니까~
    - Thymeleaf : html을 만들어주는 템플릿 엔진 필요
  - generate 누르면 프로젝트가 다운 된다. 프로젝트 압축을 풀고 InteliJ에서 해당 경로의 build.gradle 선택 -> Open as Project
- 외부에서 다운 받기 때문에 프로젝트 생성에 좀 오래걸린다... 정말 오래걸리네^^ㅠㅠ 밥먹고 오자
- src에 main, test 폴더가 자동으로 만들어져 있는데 테스트코드의 중요성을 알 수 있다.
- resources는 실제 자바 파일 코드 제외한 xml, properties, 설정파일, html 등이 들어감.
- build.gradle 파일이 중요함. 옛날에 이걸 다~ 쳤어야 했는데 부트가 만들어준다!
  - `sourceCompatiablity = 11` : 자바 11버전
  - `repositories{mavenCentral()}` : dependencies 다운로드 여기서 받아라 설정 간편하게 한 것
- main의 HelloSpringApplication.java를 실행시키면 8080 포트를 이미 쓰고있다고 에러가 뜨는데 resources/application.properties에 `server.port = 8090` 추가해주면 해결 된다.

- ```java
  @SpringBootApplication
  public class HelloSpringApplication {

    public static void main(String[] args) {
      SpringApplication.run(HelloSpringApplication.class, args);
    }

  }
  ```

  main의 SpringApplication.run 을 통해 스프링 애플리케이션이 실행 됨. 스프링을 띄우면서 밑에 톰캣 같은 웹서버를 내장하고 있는데 그 웹서버를 자체적으로 띄우며 스프링부트가 같이 올라오는 원리임.

- 인텔리제이를 쓰면 gradle 통해 실행되는 게 기본설정이다. Settings-gradle-Build and run using과 Run tests using을 InteliJ IDEA로 바꿔주자. 그럼 인텔리제이에서 바로 자바 띄워서 돌려서 훨씬 빨라진다

## 라이브러리 살펴보기

- gradle, maven 같은 빌드 툴들은 의존관계가 있는 라이브러리를 함께 다운로드 한다.
  - spring-boot-starter-web을 가져오면 스프링 웹에 관련된 dependencies를 다 같이 가져온다. 그렇게 가져온 애들이 또 의존 된 애들을 가져오고... 이렇게 의존관계를 받아오다 보니 build.gradle 엔 세줄밖에 안 적혀 있는데 External Libraries 보면 엄~청 많이 다운받아져 있음을 확인 가능.

**스프링부트 라이브러리**

- spring-boot-starter-web

  - spring-boot-starter-tomcat : 톰캣(웹서버)

    옛날엔 웹서버를 서버에 설치(Tomcat)하고 거기에 자바 코드를 밀어넣는 식으로 해서 웹서버와 개발 라이브러리가 분리되어 있었는데 요즘은 소스 라이브러리에서 이런 웹서버가 임베디드(내장) 되어있음. 자바 코드만 실행했는데 웹서버가 뜨고 8090으로 들어갈 수 있고... 요즘은 다 이렇게 함! 그냥 라이브러리 빌드해서 웹서버에 올리면 된다.

  - spring-webmvc : 스프링 웹 MVC

- spring-boot-starter-thymeleaf : 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통) : 스프링부트 + 스프링코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback, slf4j  
      system.out.print가 아닌 logging을 사용해야 심각한 에러만 따로 모아보고 로그 파일만 관리할 수 있다.

**테스트 라이브러리**

- spring-boot-starter-test
  - junit : 테스트 프레임워크, 최근엔 junit5를 많이 쓴다.
  - mockito : 목 라이브러리
  - assertj : 테스트코드 작성을 좀 더 편하게 해주는 라이브러리
  - spring-test : 스프링 통합 테스트 지원

## View 환경설정

- 스프링의 기능 어마무시하게 많은데 필요 기능 찾는 능력이 중요함 : spring.io -> Project -> Spring boot -> Learn -> Reference Doc.
- 스프링 부트가 제공하는 Welcome Page 기능

  - static/index.html을 올리면 Welcome Page가 기능  
    https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page

- Thymeleaf 템플릿 엔진

  - 내가 원하는대로 정적 페이지를 동적 페이지로 바꿀 수 있다.
  - 공식 사이트 : https://www.thymeleaf.org/
  - 스프링 공식 튜토리얼 : https://spring.io/guides/gs/serving-web-content/
  - 스프링부트 메뉴얼 : https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-template-engines

- localhost:8080/hello로 들어가면 내장 톰켓 서버에서 스프링 helloController에 있는 hello로 url매핑한 hello메서드가 실행된다. 스프링이 model 을 만들어서 넣어주고 addAttribute로 "data"에 값 "hello!!"를 넣어줬다. 그리고 return을 "hello"로 하면 스프링부트는 resources/templates 의 "hello.html"를 찾는다(리소스를 찾아서 랜더링을 한다). 컨트롤러에서 리턴 값으로 문자를 반환하면 viewResolver가 화면을 찾아 처리해준다.

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.

  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources:templates/`+{ViewName}+`.html`

- `spring-boot-devtools` 라이브러리를 추가하면 `html` 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.

- 웹 애플리케이션 렌더링 방식을 잘 정리해주셨다. https://www.inflearn.com/questions/72824 렌더링 방식이 서버 사이드/클라이언트 사이드로 나뉘는지 처음 알았다.. 강사님이 Q&N 도 너무 성실하게 답해주셔서 강의 다 보고 큐앤에이 한바퀴 쭉 둘러보게 된다ㅋㅋ 너무 좋아~~!!ㅠㅠ

- ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
    <head>
      <title>Hello</title>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    </head>
    <body>
      <p th:text="'안녕하세요. ' + ${data}">안녕하세요. 손님</p>
    </body>
  </html>
  ```
  서버로 실행시켰을 때 "안녕하세요. 손님" 부분은 출력이 안 되는데 thymeleaf 뷰 템플릿이 갖고 있는 기능 때문에 그렇다.  
  "안녕하세요. 손님"은 서버가 없을 때 나오는 기본값이라고 보면 되겠다. 서버가 동작하면 th:text부분이 동작해서 "안녕하세요. +데이터값"이 나온다. 즉 th:text의 값이 기본 값 내용을 변경한다.
  jsp 같은 경우엔 서버 없이 파일만 열면 프로그래밍 로직이 안에 들어있어서 html이 깨져보인다. 하지만 thymeleaf는 위와 같은 메커니즘으로 html파일만 열어도 기본값을 통해 결과를 볼 수 있다.

## 빌드하고 실행하기

(윈도우기준) 인텔리제이에서 서버를 끄고 콘솔로 이동해서

1. gradlew.bat 실행(cmd에서 gradlew 엔터)
2. cd build/libs
3. java -jar hello-spring-0.0.1-SNAPSHOT.jar
4. 실행 확인
5. 컨트롤 + c를 누르면 서버종료

- 서버 배포할 땐 이 파일만 복사해서 서버에 넣어서 java -jar로 실행시켜주면 된다. 그렇게 하면 서버에서도 스프링이 동작하게 된다.

- 잘 안 될땐
  - gradlew clean : 빌드 폴더가 사라진다
  - gradlew clean build : 완전히 지우고 다시 빌드한다
