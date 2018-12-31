---
layout: post
title: "AWS의 ELB과 연결되어 있는 인스턴스에 HTTP로 접속 시 HTTPS로 접속하는 방법"
date: 2018-10-13 23:59:59 +0900
tags: [aws, elb, ssl, https]
comments: true
---
## AWS와 HTTPS 프로토콜 사용
요즘 IT회사들 특히, 스타트업(Startup)에서는 AWS(Amazon Web Service)와 같은 IaaS(Infrastructure as a Service)를 많이 활용하고 있다.

거기에다가 요즘은 HTTP가 아닌 HTTPS 프로토콜(Protocol)을 이용하여 접속하는 사이트들을 많이 접할 수 있다.

HTTPS 프로토콜을 사용하는 이유는 [이 사이트](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/why-https?hl=ko)에서 자세히 설명하고 있다.

## 이 블로그 글은 왜 필요한가?
보통 우리가 사용하는 웹브라우저(인터넷 익스플로러, 크롬 등)에서 주소창에 주소를 입력하면 HTTP 프로토콜을 사용하게 된다.

예를 들어, 주소창에 ‘google.com’을 입력하면 웹브라우저는 ‘http&#58;//google\.com’ 주소로 이동하게 된다.

### 아니, 근데 왜 거짓말을 하시죠? ‘https&#58;//www.google\.com’으로 접속되는데요!
라고 말하시는 분이 계실 것이다. 이렇게 대답하셨던 분은 이 블로그 글을 꼭 읽어보시길 바란다.
우리가 최종적으로 해야 할 목표이기도 하다.

오늘 설명할 내용은 AWS의 ELB(Elastic Load Balancer)과 연결되어 있는 인스턴스(Instance)에 HTTPS로 HTTP 트래픽 리디렉션(Redirection)하는 방법을 설명하겠다.

아마 HTTPS를 사용하는 사이트에 필수적으로 적용하는 것이라 매우 유용한 글이 될꺼라 생각한다.

(AWS의 ELB를 사용하고 ELB에 SSL 인증서를 적용한 이후의 상황이라고 가정하고 설명하겠다.)

![ELB](/files/redirect-http-https-elb-01.png)

보통 회사에서의 서버 구성을 간단하게 나타내면 위와 같은 그림일 것이다.

로드밸런서(Load Balancer)를 사용하는 이유를 설명하면 글이 길어질거 같아서 생략하겠다.

(혹시 이에 대해 궁금하다는 댓글이 많다면, 따로 설명하는 글을 작성하도록 하겠다.)

AWS에서는 두 가지 형태의 로드밸런서를 제공하고 있다.
- Classic Load Balancer
- Application Load Balancer

만약 Application Load Balancer를 사용하고 있다면 지금 뒤로가기 버튼을 눌러도 좋다.

왜냐하면 Application Load Balancer는 간단하게 AWS 대시보드에서 리디렉션 작업이 가능하기 때문이다. [[링크]](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/application/load-balancer-listeners.html#redirect-actions)

Application Load Balancer 서비스가 공개된 2016년 8월 12일 이전([[링크]](https://aws.amazon.com/ko/blogs/korea/new-aws-application-load-balancer/))에 AWS ELB를 사용했다면 100% Classic Load Balancer 사용자이다.

우리 회사도 예외가 아니었다. 당첨!

번거롭지만, 앞으로 설명드릴 내용들을 여러분들의 톰켓(Tomcat) 서버에 설정해야 한다.

## 3일간의 노력한 결과를 여러분들께 공개합니다!
제발 이 글을 읽으신 분들은 시간낭비 하지 마시길 바라는 마음뿐입니다. ㅠㅠ

최종적인 목표는 웹브라우저 주소창에 ‘wdgbook\.com’, ‘www\.wdgbook\.com’을 입력하면

자동으로 ‘https&#58;//www\.wdgbook\.com’으로 리디렉션되게 하는 것이다.

## 1. 처리한 내용
(1) 작업할 인스턴스의 콘솔(Console)에 접속하기 (PuTTY 등을 활용)

(2) AWS 대시보드 접속하여 로드밸런서에 붙어있는 인스턴스 하나 제거 (설마 인스턴스 하나만 있는 건 아니겠죠?)

(3) Tomcat의 server.xml 파일이 있는 경로로 이동 (보안상 자세한 경로는 생략)

(4) VI 편집기를 이용하여 server.xml 파일 열기 (#vi server.xml)

(5) 아래와 같은 내용을 AccessLogValve 바로 위에 추가한 뒤 저장 [[링크]](https://stackoverflow.com/questions/5741210/handling-x-forwarded-proto-in-java-apache-tomcat)

```xml
<Valve className="org.apache.catalina.valves.RemoteIpValve"
          internalProxies="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"
          remoteIpHeader="x-forwarded-for"
          proxiesHeader="x-forwarded-by"
          protocolHeader="x-forwarded-proto" />
```

(6) rewrite.config 파일이 있는 경로로 이동. (만약 파일이 없다면 ‘/WEB-INF’ 폴더에 새로 생성)

(7) VI 편집기를 이용하여 rewrite.config 파일 열기 (#vi rewrite.config)

(8) 아래와 같은 내용으로 작성 후 저장 (작성법은 [[링크]](https://tomcat.apache.org/tomcat-9.0-doc/rewrite.html) 참고)

```sh
RewriteCond %{HTTP:X-Forwarded-Proto} =http
RewriteCond %{HTTP_HOST} ^(www\.wdgbook\.com|wdgbook\.com)
RewriteRule .* https://www.wdgbook.com%{REQUEST_URI} [L,R=301]
```
(9) Tomcat 서버 shutdown 하기

(10) Tomcat 서버 startup 하기

(11) 로드밸런서에 1-(2)번에서 제거했던 인스턴스 연결하기

## 2. 위와 같이 처리한 이유
(1) AWS ELB를 통해서 들어온 요청(Request)의 경우, Tomcat의 RewriteCond에서 %{HTTPS} 값을 알 수가 없다.

(2) (1)의 문제를 해결하기 위해서는 HTTP 요청의 X-Forwarded-Proto 헤더를 사용해야 한다고 AWS 가이드에 설명되어 있다. [[링크]](https://aws.amazon.com/ko/premiumsupport/knowledge-center/redirect-http-https-elb/)

(3) HTTP 요청의 X-Forwarded-Proto 헤더를 활성화 하기 위해서는 1-(5)번과 같이 Tomcat 설정을 해줘야 한다.

internalProxies 항목은 직접 로드밸런서의 IP주소를 작성하거나 1-(5)번 처럼 “\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}” 값을 사용하면 인스턴스와 연결된 모든 로드밸런서를 뜻하는 것이다. 

(로드밸런서 IP주소가 유동적으로 변경될 수 있기 때문에 이렇게 처리를 했다.)

(4) 1-(8)번의 내용은

프로토콜이 http이고

요청한 호스트의 주소가 www\.wdgbook\.com 이거나 wdgbook\.com 이면

https&#58;//www\.wdgbook\.com 으로 Redirect 되도록 응답을 301(Permanent Redirect)을 주도록 설정했다.

301로 응답하는 이유는 검색엔진에게 말 그대로 ‘영구적’으로 옮겼다는 뜻으로 해석하게 만들어 검색엔진이 우리 사이트에 대한 정보를 최적화 SEO(Search Engine Optimization, CEO 아님 ^^) 하도록 도와줄 수 있다.