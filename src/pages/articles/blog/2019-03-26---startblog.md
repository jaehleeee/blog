---
title: Gatsby로 블로그 시작하기 with Netlify
date: "2019-03-26"
layout: post
draft: false
path: "/posts/startblog"
category: "blog"
tags:
  - "gatsby"
  - "blog"
  - "markdown"
description: "우여곡절 끝에 블로그를 만들었다."
---

## 블로그 하기로 결정
개발 블로그를 하기로 했다.  
원래 네이버 블로그로 오만 잡다한 포스팅을 하긴 했다.
그런데 뭔가 스스로 좀 더 개발자스러운 개발자가 되기 위해서 네이버 블로그는 아닌 것 같았다.
그러다 [초보 몽키님의 블로그](https://wayhome25.github.io/)를 우연히 보게 되었는데,
매일같이 TIL 올리시고 공부하신거 포스팅하시는 것이 너무 멋있었다.
그래서 나도 블로그를 시작하기로 결심했다.
      
  
  
## 어떻게 만들지?
많은 사람들이 Jekyll 이라는 static site generator를 통해서 블로그를 만든다는 것을 알게 됐다.
하지만 Jekyll 내부적으로 `Ruby`를 쓰기 때문에 나중에 커스터마이징하기 힘들 것 같았다.
그래서 뒤져본 결과 `React`가 사용된 generator인 **Gatsby** 를 발견하게 되었고 요걸로 블로그를 만들기로 했다.
(물론 리액트는 아직 잘 모른다...나중에 배워서 꼭 커스터마이징해야지)
      
  
  
## 설치 및 블로그 시작
`React`를 잘 모르는 내게, 다행히도 Gatsby 블로그 테마 [gatsby starter](https://www.gatsbyjs.org/starters/?v=2) 가 있었고,
여기서 깔끔해보이는 [lumen](https://lumen.netlify.com/) 테마로 결정했다. 이후 cmd창에서 아래 코드를 하나씩 진행시키면 된다.
(난 git bash에서 진행했다)
  
```
npm install --global gatsby-cli
gatsby new [디렉토리명] [테]마URL]
cd [디렉토리명]
npm install
```

내 경우,  
[디렉토리명]에  *jaegoon*  
[테마URL]에 *https://github.com/alxshelepenok/gatsby-starter-lumen.git*  
를 넣었다.

그리고 `gatsby develop` 명령을 통해 localhost:8000 에서 확인 가능하다.
  
  
## 배포
배포는 [github pages](https://pages.github.com/)나 [netlify](https://www.netlify.com/)를 통해 할 수 있는데, [netlify](https://www.netlify.com/)가 더 간단한 것 같아 바로 진행했다. 순서는 다음과 같다.
1. 본인의 깃허브에 원하는 이름으로 repository를 만든다.
2. [디렉토리명]으로 복사해온 블로그 소스들을 전부 rerepository에 넣는다.
3. [netlify](https://www.netlify.com/)에 들어가서 깃허브 & rerepository를 연결시킨다.
4. 이후부터 git에 push할때마다 자동 수정 배포된다.
  
  
## 앞으로...
아직 markdown에도 익숙하지 않고, 블로그도 손볼 곳이 많지만 하나씩 발전해 가겠다.