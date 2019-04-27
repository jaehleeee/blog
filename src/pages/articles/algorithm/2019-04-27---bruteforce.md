---
title: chap 6. 무식하게 풀기(brute force)
date: "2019-04-27"
layout: post
draft: false
path: "/posts/bruteforce/"
category: "ALGORITHM"
tags:
  - "ALGORITHM"
  - "brute force"
description: "brute force, 즉 완전탐색에 대해 정리해보자."
---

>종만북, JM북이라고 알려진 구종만님의 *알고리즘 문제 해결 전략* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.


## 1. 무식하게 풀기(brute force)란?
전산학에서 컴퓨터의 빠른 계산 능력을 이용해 가능한 경우의 수를 일일이 나열하면서 답을 찾는 방법을 의미한다.
brute는 짐승? 무식? 정도의 의미이고 force는 힘? 침투? 정도로 해석되니, 합쳐서 자연스럽게 해석해보면 **무차별 대입** 정도로 해석할 수 있겠다.
이를 알고리즘적으로 표현하면 **완전 탐색** 이라 할 수 있다.
현실 세계의 문제 중에는 입력의 크기가 작아 컴퓨터가  처리하기에는 별 것 아니지만 손으로 직접 풀기에는 경우의 수가 너무 많은 경우가 종종 있는데,
이럴 때 완전 탐색은 충분히 빠르면서도 가장 구현하기 쉬운 대안이 된다.

- 기저사례  
완전 탐색을 하는 과정에서 같은 작업이 작고 반복적인 작업으로 나뉠 수 있는데 이때 재귀 함수(recursive function)를 이용한다.
그리고 재귀 함수는 '더 이상 쪼개지지 않는' 최소한의 작업에 도달했을 때 답을 곧장 반환하는 조건문을 포함해야 한다.
이런 가장 작은 작업을 가리켜 재귀 호출의 기저 사례(base case)라고 한다.

- 풀이 방법  
모두 방문해본 결과를 모아 정답을 리턴한다.

```
int dfs(int param, boolean visit[]){

  // 1. 기저사례
  if(param < 0 || N < param) return false;

  // 2. 종료조건
  if(모두 방문) return 1;

  // 3. 메모이제이션 - 필요시
  if(cache[param] != -1) return cache[param];

  // 4. 풀이(recursive 활용) - 모두 방문한 결과를 temp에 저장해 return 한다.
  int tempAnswer = 0;
  for(int i=0; i<N; i++){
    visit[i] = true;
    tempAnswer += dfs(i, visit);
    visit[i] = false;
  }

  // 5. 메모이제이션 저장 - 필요시
  return cache[param] = tempAnswer;
}

```
  

## 2. 최적화 문제 (Optimization problem)
문제의 답이 하나가 아니라 여러 개이고, 그 중에서 어떤 기준에 따라 가장 '좋은' 답을 찾아 내는 문제들을 통칭해 최적화 문제라고 부른다.



## 3. 완전탐색 문제 유형
1. 순열
서로 다른 N개의 원소 줄세우기 - N!
2. 조합
서로 다른 N개 원소 순서에 관계없이 R개 고르기
3. 2^N 경우의 수
N개의 질문에 예/아니오로 답할 수 있을 때, 대답할 수 있는 모든 경우의 수

