---
title: 스프링 componentScan
date: "2019-04-05"
layout: post
draft: false
path: "/posts/componentScan/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링 componentScan에 대해서 알아본다."
---

>최범균님의 *스프링 프로그래밍 입문* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.

### 목차
1. componentScan 이란
2. 특정 어노테이션 스캔 대상 제외, 포함하기
3. componentScan에 따른 충돌
4. annotation-config? annotation-driven?

<br><br>
## 1. componentScan 이란
지난 포스팅에서 @Autowired를 통해 객체를 자동으로 주입하는 것에 대해 배웠다.
이번엔 주입이 아니라 객체 자체를 자동으로 만들어주는 기능이 있다.  
바로 componentScan이다. 사용방법은 간단하다. 
스프링 컨테이너에 componentScan이다 대상 패키지를 설정하고 패키지 내 객체로 자동 생성할 클래스에 @Component 어노테이션을 붙이면 된다.

#### 1. 컨테이너 설정 (Application Context 파일)
- .java일 경우 `@ComponentScan(basePackages={"com.pjt.spring"})` 어노테이션을 클래스명 위에 붙이거나
- .xml일 경우 `<context:component-scan base-package="com.pjt.spring" />` 태그를 추가하면 된다.

여기서 basePackages는 componentScan을 진행할 대상 패키지를 정하는 것이다.
여러개일 경우, 콤마로 구분하고 상위 패키지도 한번에 추가할 수 있다.

#### 2. Component 지정
자동으로 객체 생성하길 원하는 클래스에 알맞는 어노테이션을 붙이면 된다.
@Component
@Controller
@Service
@Repository


<br><br>
## 2. 특정 어노테이션 스캔 대상 제외, 포함하기
include-filter 태그를 이용하면 해당되는 어노테이션을 스캔 대상에서 포함할 수 있고
exclude-filter 태그를 이용하면 해당되는 애노테이션을 스캔 대상에서 제외할 수 있다.
해당 태그를 `<context:component-scan>` 태그 안에 넣으면 된다.
아래 예시는 Controller 어노테이션을 스캔 대상에서 제외한다는 설정이다.
```
<context:component-scan base-package="com.pjt.spring">
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
</context:component-scan>
```


<br><br>
## 3. componentScan에 따른 충돌
1. 빈 이름 충돌 - 에러 발생  
base-packages로 spring1, spring2 를 등록하고
두 패키지 모두에 같은 이름의 클래스가 있고 @Component 어노테이션이 붙어있다면 어떻게 될까.
`BeanDefinitionStoreException`이 발생한다. 따라서 명시적으로 bean 이름을 지정해서 충돌을 피애햐 한다.
  
2. 수동 등록한 빈과 충돌 - 수동 등록한 빈이 우선  
하나의 클래스를 스프링 컨테이너에 수동으로 Bean 등록하기도 하고 componentScan 대상이기도 한 경우는
수동으로 등록한 bean이 우선된다.


<br><br>
##4. annotation-config? annotation-driven?
둘다 componentScan과 비슷하게 생겨서 헤깔리기도 하고 궁금하기도 해서 간단히 정리한다.

1. context:annotation-config  
이미 등록된 Bean에  @Configuration, @Required, @Autowired 등의 어노테이션 기능을 활성화해준다.
이는 componentScan 기능에 포함된 기능이기 때문에 componentScan 설정이 되어 있다면 필요없다.

2. mvc:annotation-driven  
HandlerMapping, HandlerAdapter 두 클래스를 빈으로 등록한다.
HandlerMapping : 요청 URL과 매칭되는 Controller를 찾아준다.
HandlerAdapter : Controller 실행 결과를 리턴한다.
