---
title: webservice에 대해여 (기초)
date: "2019-04-17"
layout: post
draft: false
path: "/posts/webservice"
category: "common"
tags:
  - "webservice"
description: "webservice를 공부해보자."
---

회사 프로젝트에서 웹서비스를 할 때 soap 방식을 이용한다고 한다.
평소 REST API에 대해서도 궁금하던 찰나 관련해서 공부해보고자 한다.

## 1. 웹서비스란?
Microsoft에서 정의한 웹서비스 설명이 이해하기 쉽고 명료해서 설명을 대신한다.
>웹 서비스는 다른 어플리케이션에 데이터와 서비스를 제공하는 어플리케이션 로직의 단위이다.


## 2. Web Service 종류
웹서비스는 SOA(Service Oriented Architecture)를 지향하는 SOAP 방식과 ROA(Resource Oriendted Architecture)를 지향하는 REST 방식이 있다.
웹서비스 초기에 SOAP가 각광받았지만 개발 난이도가 높고 느리다는 단점 때문에 최근 REST 방식이 각광받고 있다.
하지만 내가 일하는 곳에서는 아직 SOAP 방식, 그 중에서도 CXF 방식을 사용하고 있기 때문에 이에 대해 정리하고자 한다.
(CXF는 웹서비스 프레임워크로, 다른 프레임워크로 AXIS, Spring-XS가 있음)

SOAP의 가장 큰 특징은 UDDI라는 웹서비스 오픈 마켓이 있다는 점이다. 이 곳에 웹서비스 명세가 담겨진 WSDL이 있고
클라이언트들은 여기서 WSDL을 통해 웹서비스가 가능하다.
 
*용어
- REST : Representational State Transfer
- SOAP : Simple Object Access Protocols
- WSDL : Web Service Description Language
- UDDI : Universal Description and Discovery and Integration
 
 
## 3. Web Service 클라이언트/서버 선정 방식
데이터를 요청하는 수신자와 데이터를 보내주는 송신자가 있다.
이 경우, 당연히 데이터를 보내는 쪽이 서버가 되어야 할 것 같지만,
경우에 따라서 수신자가 서버가 되기도 하다.

1) 수신자가 서버가 되는 경우 (현재 채택)
 - 수신자가 데이터를 보낸다. (데이터송신 자체가 요청이 되고 송신자가 클라이언트가 된다.)
 - 송신자는 데이터를 받을 서버를 만들고 데이터를 받은 후 인터페이스 성공 여부를 보낸다.
 - 수신자가 인터페이스 성공 여부를 받는다.
 - 보통 송신자 측에서 서버를 생성할 여건이 안되는 경우 사용하는 방식이다.

2) 송신자가 서버가 되는 경우
 - 수신자가 데이터 달라고 수신측에 요청을 보낸다.
 - 송신자가 서버를 만들고 데이터를 보낸다.
 - 수신자가 데이터를 받는다.



참고
http://www.infopub.co.kr/info/ebook/pdf/8054-490.pdf
http://www.nextree.co.kr/p11842/
http://www.nextree.co.kr/p11410/