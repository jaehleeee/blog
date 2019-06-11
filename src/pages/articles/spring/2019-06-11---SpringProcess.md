---
title: 스프링 MVC 프레임워크 동작 과정
date: "2019-06-11"
layout: post
draft: false
path: "/posts/SPRING-PROCESS/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링 MVC 프레임워크 동작 과정과 원리에 대해 알아보자"
---

>최범균님의 *스프링 프로그래밍 입문* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.


<-- 1. 서버 실행 -->

### 1. WAS가 구동되면 web.xml를 로딩한다.
### 2. 로딩된 web.xml에서 ContextLoaderListener 클래스를 생성한다.
### 3. 생성된 ContextLoaderListener는 root-context.xml을 로딩한다. 이때 root-context는 Context-param으로 선언되었기 때문에 WebApp 전역적으로 사용된다.
### 4. 로딩된 root-context에 등록된 Spring 컨테이너가 구동되며 Service, dao 등의 객체도 함께 생성된다.


<-- 2. 클라이언트로부터 요청이 왔을 때 -->

### 5. DispatcherServlet이 구동된다. 이때 Servlet으로 선언되었기 때문에 해당 Servlet 내에서만 사용가능한 Context가 된다.
### 6. DispatcherServlet이 servlet-context.xml을 로딩한다.
### 7. 로딩된 servlet-context에 등록된 Spring 컨테이너가 구동되며 등록된 Controller 객체를 생성한다.


<-- 3. 클라이언트 요청 처리 -->
### 8. 요청 URL을 DispatcherServlet이 가로챈다.
### 9. HandlerMapping 객체가 알맞은 Controller 객체를 찾은 후 DispatcherServlet에게 전달한다.
### 10. DispatcherServlet은 Controller 종류에 따라 똑같은 방식으로 처리하도록 만들어졌고 이를 가능하게 하는 것이 HandlerAdapter이다. HandlerAdapter는 HandlerMapping이 찾은 컨트롤러의 적절한 메서드를 실행시킨 후 결과를 ModelAndView 객체로 변환해 DispatcherServlet에게 전달한다. 
### 11. DispatcherServlet이 ModelAndView 객체를 받으면 ViewResolver 객체가 화면에 표시할 뷰 객체를 찾아 DispatcherServlet에 리턴한다.
### 12. DispatcherServlet에 뷰 객체가 리턴되면 뷰 객체에 응답 결과 생성을 요청한 후 화면에 나타낸다.