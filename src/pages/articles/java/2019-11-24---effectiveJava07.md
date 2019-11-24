---
title: 다 쓴 객체 참조를 해제하라.
date: "2019-11-24"
layout: post
draft: false
path: "/posts/java/effective07/"
category: "JAVA"
tags:
  - "java"
description: "[Effective Java 3/E - 아이템7] 다 쓴 객체 참조를 해제하라."
---
자바는 GC가 알아서 다 쓴 객체를 지워주기 때문에 직접 메모리를 관리하는 언어보다 훨씬 편하게 개발할 수 있다. 하지만 가끔은 메모리 누수가 발생해 GC 활동과 메모리 사용량이 증가해 성능이 저하될 수 있다. Stack 클래스를 예시로 살펴보자.

```
public class Stack {

    private Object [] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public void push(Object e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if(size == 0) {
            throw new EmptyStackException();
        } else {
            return elements[--size];
        }
    }

    private void ensureCapacity() {
        if(elements.length == size) {
            elements = Arrays.copyOf(elements, 2*size + 1);
        }
    }
}

```
위 클래스에서 배열의 크기가 커졌다가 줄어들었다가 하는 과정에서 꺼내어진 객체들은 GC가 회수하지 않는다. 프로그래머들이야 더 이상 사용되는 객체인지 아닌지 알 수 있지만 GC는 알 수 없기 때문이다. 따라서 더 이상 사용되지 않는 개체는 참조를 해제해야 한다. 해제 방법은 간단하다. null 처리하면 된다.
 + null 처리시 추가적인 효과도 있다. null 처리한 객체를 실수로 사용하려고 한다면 에러를 발생시켜 오류를 빠르게 발견할 수 있다.

```
public Object pop() {
    if(size == 0) {
        throw new EmptyStackException();
    } else {
        Object result = elements[--size];
        elements[size] = null;             // null 처리
        return result;
    }
}
```

물론 모든 경우마다 null 처리할 필요는 없다. 코드만 지저분해질 뿐이다. *자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시메모리 누수에 주의해야 한다.*