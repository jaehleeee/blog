---
title: 스프링 @Autowired
date: "2019-04-02"
layout: post
draft: false
path: "/posts/Autowired/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링 @Autowired에 대해서 알아본다."
---

>최범균님의 *스프링 프로그래밍 입문* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.

### 목차
1. @Autowired란?
2. @Autowired 사용 방법 3가지
3. @Autowired 대상 Bean이 없는 경우 - required 속성, Optional, @Nullable
4. @Autowired 대상 Bean이 여러 개인 경우 - @Primary, @Qualifier
5. 자동 주입과 명시적 의존 주입 간의 관계

<br><br>
## 1. @Autowired란?
현재 설정 파일이나 클래스에서 객체를 의존 주입 할 때
다른 설정 파일에서 생성된 객체를 자동으로 의존 주입한다.
의존 주입 기능을 가진 어노테이션은 `@Autowired` 외에도 `@Resource`와 `@Inject` 가 있다.

<br><br>
## 2. @Autowired 사용 방법 3가지
`@Autowired`는 필드나 메소드 둘다에게 붙일 수 있기 때문에 다음과 같은 3가지 방법이 있다.
- 필드
- 생성자
- setter 메소드  

위 3가지 방법 모두 설정 파일에서 따로 의존 주입할 필요 없다.

#### 필드
```
class Phone{

  @Autowired
  Battery battery;

  battery.turnOn();

}
```

#### 생성자
```
class Phone{

  
  Battery battery;
  
  @Autowired
  public Phone(Battery battery){
    this.battery = battery;
  }

}
```

#### setter 메소드
```
class Phone{

  
  Battery battery;
  
  @Autowired
  public setBatter(Battery battery){
    this.battery = battery;
  }

}
```

<br><br>
## 3. @Autowired 대상 Bean이 없는 경우 - required 속성, Optional, @Nullable
@Autowired 사용시 자동 주입할 대상 Bean이 없는 경우 에러가 발생한다.
이를 방지하기 위해서는 `@Autowired`에 required 속성을 부여하거나 파라미터에 `Optional`, `@Nullable`을 붙여 해결할 수 있다.
Autowired할 위치에 `@Autowired(required=false)`로 설정하면 대상 bean이 없을 경우 의존 주입하지 않는다.
또한 `Optional`이나 `@Nullable`을 의존 객체 파라미터 앞에 붙이면 같은 기능을 한다.
```
class Phone{

  
  Battery battery;
  
  @Autowired
  public setBatter(@Nullable Battery battery){
    this.battery = battery;
  }

}
``` 
> Optional의 경우 `@Nullable Battery battery` 부분을 `Optioal<Battery> battery`처럼 사용한다.

`required 속성`과 `@Nullable`의 차이점은 메소드 실행 여부에 있다.
`required 속성`은 메소드 자체를 실행하지 않는 반면, `@Nullable`은 null 값을 할당하여 메소드를 실행한다.

<br><br>
## 4. @Autowired 대상 Bean이 여러 개인 경우 - @Primary, @Qualifier
자동 주입 대상 Bean이 여러 개인 경우 자동 주입할 Bean을 선택할 방법이 필요하다.
2가지 방법이 있다.
- @Primary : 이 어노테이션이 붙은 Bean을 우선적으로 주입한다.
- @Qualifier("한정자") : 한정자라는 별명이 붙은 Qualifier 어노테이션을 Bean과 Autowired에 붙여 주입할 bean을 선택한다.

<br><br>
## 5. 자동 주입과 명시적 의존 주입 간의 관계
명시적 의존 주입과 @Autowired를 이용한 자동 주입이 모두 설정된 경우, 명시적 의존 주입이 우선적으로 반영된다.
