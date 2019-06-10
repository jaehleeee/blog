---
title: 스프링 Controller에서 parameter 처리 방법
date: "2019-05-23"
layout: post
draft: false
path: "/posts/param/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링 Controller에서 parameter를 처리하는 방법에 대해서 알아본다."
---

## 1. HttpServletRequest 객체
클라이언트 정보를 비롯하여 쿠키, 헤더, GET/POST로 전송한 parameter 정보를 전달한다.
```
@RequestMapping("url/url")
retrieveMethod(HttpServletRequest request){
	String userName = request.getParameter("userName");
	...
}
```

## 2. @RequestParam, 단일 변수 매핑
RequestParam는 클라이언트가 요청시 호출한 param과 controller에서 매핑할 변수를 1:1로 매핑한다.

```
@RequestMapping("url/url")
retrieveMethod(@RequestParam(value="name", required = false, defaultValue="jaejae") String name){
	String userName = name;
	...
}
```

## 3. @RequestParam, Map 자료구조 매핑
```
@RequestMapping("url/url")
retrieveMethod(@RequestParam HashMap<string, string> map){
	String userName = map.get("userName")
	...
}
```

## 4. 커맨드 객체
RequestParam처럼 변수를 1:1로 매핑하는 것이 아니라
DTO 같은 객체의 프로퍼티에 변수를 매핑해서 한꺼번에 Controller로 가져온다.

스프링에서 파라미터 변수명과 객체의 변수명이 같으면
파라미터를 자동으로 매핑시켜 준다.

```
@RequestMapping("url/url")
retrieveMethod(User user){
	String userName = user.getUsername();
	...
}
```

## 5. @ModelAttribute
커맨드 객체의 이름이 너무 길 경우 view 단에서 사용하기 어려울때
커맨드 객체가 view 단에서 사용될 이름을 지정한다.
생략가능하다.
```
@RequestMapping("url/url")
retrieveMethod(@ModelAttribute("us") User user){
	String userName = user.getUsername();
	...
}
// 이후 view 단에서 us.userName 으로 사용 가능.
```


### 참고
- https://heavenly-appear.tistory.com
