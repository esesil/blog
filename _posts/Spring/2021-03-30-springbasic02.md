---
title: "[인프런]스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 02"
date: 2021-03-30 화요일 23:00:00+0900
excerpt: "스프링 웹 개발 기초"
categories:
  - Spring
tags:
  - Spring
toc: true
toc_sticky: true
last_modified_at: 2021-03-30T08:06:00-05:00
---

> 인프런, 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 강의를 듣고 정리한 내용입니다.

- 정적 컨텐츠는 서버 없이 파일을 웹브라우저에 전송하는 것
- MVC, 템플릿 엔진 : 템플릿 엔진을 통해 동적페이지를 웹브라우저에 보여준다, 이걸 위해 컨트롤러/모델/뷰 MVC를 사용한다.
- API : json이라는 데이터 구조 포맷으로 클라이언트에게 데이터를 전달함, 뷰/리액트 등을 쓸 때도 API로 데이터 주면 화면은 클라이언트가 알아서 그리고 구현하는 API방식 사용, 서버끼리 통신 할 때 html을 내릴 필요 없으니 API방식 사용

## 정적 컨텐츠

- Static Content
- static 폴더에 html 만들고 주소로 http://localhost:8090/html파일명.html로 들어가면 html 파일 확인 가능, 이걸 정적 컨텐츠를 제공하는 기능이라고 함
- 원리는 html파일명으로 매핑된 컨트롤러 있는지 확인 후 없으면 static에서 그 이름의 html파일을 찾아준다.

## MVC와 템플릿 엔진

- MVC : Model, View, Controller
- 모델 1은 jsp에 모든걸 다 작성한 것, 모델2 기능들을 mvc패턴으로 분리한 것.
- view는 화면을 그리는데만 집중해야 하고 model, controller는 비즈니스 로직/내부적 로직을 처리하는 데 집중해야 한다.
- Controller 메소드에 parameter를 추가해 봤다. 내장톰켓서버는 매핑된 메서드를 호출해주고 model(name:spring)을 스프링에게 넘겨주면 viewResolver가 작동한다. viewResolver가 hello-template.html 을 찾아서 Thymeleaf 템플릿 엔진 처리를 하고 **HTML로 변환 후** 웹브라우저에게 넘겨준다. (정적은 변환 없이 그대로 반환함)
- parameter가 있고 `@RequestParam("name")`에 `requried=false`가 아니니 http://localhost:8090/hello-mvc?name=spring 이렇게 url 뒤에 파라미터 붙여줘야 정상작동함~! `(@RequestParam(value="name",required = false) String name, Model model)` 였다면 파라미터 안 붙여줘도 hello null 라는 텍스트로 페이지가 작동한다.

## API

- 정적 컨텐츠 제외하면 ① mvc 방식으로 뷰를 찾아서 화면을 렌더링해서 html을 웹브라우저에 넘겨주는 방법 / ② API로 데이터를 바로 뿌리는 방법이 있다.
- `ResponseBody` : http body부에 데이터를 직접 넣어주겠다. 데이터를 그~대로 내려준다.
- 만약 문자가 아니라 객체를 달라고 한다면... (보통 이 방식을 사용함)

  ```java
  @GetMapping("hello-api")
      @ResponseBody
      public Hello helloApi(@RequestParam("name") String name){
          Hello hello = new Hello();
          hello.setName(name);
          return hello;
      }

      static class Hello{
          private String name;

          public String getName(){
              return name;
          }
          public void setName(String name){
              this.name=name;
          }
      }
  ```

  서버 실행시키면 `{"name":"spring!"}` 처럼 json 형식으로 나온다. 스프링도 객체를 반환하고 ResponseBody를 쓰면 기본 json으로 나옴. 요즘엔 xml이 아닌 json을 씀!

- Responsebody 가 붙어있으면 viewResolver가 아니라 http에 그대로 데이터를 넘겨야겠다고 동작함. 그런데 문자가 아니라 객체라면 기본 default로 json으로 만들어 http에 반환하겠다가 기본 정책임. 따라서 HttpMessageConverter 가 동작하고 그 안에서 단순 문자면 StringConverter가 동작하고 객체면 JsonConverter가 동작한다. 그걸 웹 브라우저에 응답해준다.

정리

- @ResponseBody 를 사용
  - HTTP의 BODY에 문자 내용을 직접 반환
  - viewResolver 대신에 HttpMessageConverter 가 동작
  - 기본 문자처리: StringHttpMessageConverter
  - 기본 객체처리: MappingJackson2HttpMessageConverter  
    객체를 json으로 바꿔주는 유명 라이브러리엔 Jackson, Gson이 있다. 스프링은 Jackson을 기본으로 탑재함.
  - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음
