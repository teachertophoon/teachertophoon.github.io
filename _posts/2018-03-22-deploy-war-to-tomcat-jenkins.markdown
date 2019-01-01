---
layout: post
title: "[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (1)"
date: 2018-03-21 23:59:59 +0900
tags: [war, deploy, tomcat, jenkins]
comments: true
---
젠킨스(Jenkins)는 [CI(Continuous Integration)](https://en.wikipedia.org/wiki/Continuous_integration)를 위한 하나의 도구(Tool)이다.


CI(지속적 통합)란 간단하게 설명드리면 일정한 시간 간격을 두고 모든 개발자가 작업한 내용을 하나로 통합(Merge)하는 작업을 뜻한다. 자세한 내용은 링크한 위키피디아를 참고하기 바란다.

## 1. 아파치(Apache) 웹 서버 설치하기
자동으로 배포하는 것을 도와주는 젠킨스를 설치하기 위해서는 우선 Apache 웹 서버를 설치하고 기존 이용하고 있던 애플리케이션 서버인 Tomcat과 연결해야 한다.

아래 명령어를 실행하여 Apache 웹 서버를 서버에 설치한다.
```sh
$ sudo apt-get install apache2
$ apt-cache search mod_jk
$ sudo apt-get install libapache2-mod-jk
```

## 2. 아파치(Apache) 웹 서버와 톰켓(Tomcat) 서버 연결하기
아래 명령어를 실행하여 아래 화면과 같이 작성한다.
```sh
$ sudo vim /etc/libapache2-mod-jk/workers.properties
```
![이미지](/files/deploy-war-to-tomcat-jenkins-01.png)

아래 명령어를 실행하여 아래 화면과 같이 작성한다.
```sh
$ sudo vim /var/lib/tomcat8/conf/server.xml
```
![이미지](/files/deploy-war-to-tomcat-jenkins-02.png)

아래 명령어를 실행하여 아래 화면과 같이 작성한다.
DocumentRoot의 값은 여러분의 배포할 WAR 파일 압축이 해제되는 폴더 경로를 작성한다.
```
$ sudo vim /etc/apache2/sites-available/000-default.conf
```
![이미지](/files/deploy-war-to-tomcat-jenkins-03.png)

*000-default.conf* 파일 맨 아래쪽에 아래 화면과 같이 작성 후 저장한다. 여기까지 작성했다면 Apache 웹 서버와 Tomcat 애플리케이션 서버의 연결이 완료된 것이다.

![이미지](/files/deploy-war-to-tomcat-jenkins-04.png)

## 3. 젠킨스(Jenkins) 설치하기
아래 명령어를 이용하여 Jenkins를 서버에 설치한다.
```sh
$ wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
$ sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
$ sudo apt-get update && sudo apt-get install jenkins
```

## 4. 젠킨스(Jenkins) 환경변수 설정하기
아래 명령어를 실행한다.
```sh
$ sudo vim /etc/default/jenkins
```

아래 내용과 같이 작성한다. *HTTP_PORT*는 젠킨스가 사용할 포트번호이다. 여기서는 *8081*번을 젠킨스가 사용하도록 설정하였다.

![이미지](/files/deploy-war-to-tomcat-jenkins-05.png)

아래 명령어를 사용하여 젠킨스 서비스를 재시작 한다.
```sh
$ sudo service jenkins restart
```
## 5. 아파치(Apache) [Reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) 서버 구성하기
리버스 프록시 서버(Reverse Proxy Server)를 간단히 설명하자면 웹 애플리케이션 서버가 직접 클라이언트의 요청(Request)을 받는 형태가 아니라
리버스 프록시 서버가 클라이언트의 요청을 받고, 이 요청을 다시 리버스 프록시 서버와 연결되어 있는 웹 애플리케이션 서버에 전달한다.
그럼 웹 애플리케이션 서버가 응답(Response)을 생성하고 이것을 다시 리버스 프록시 서버가 전달받아 최종적으로 클라이언트에게 응답을 하게 된다.

여기서 리버스 프록시 서버를 사용하는 이유는 젠킨스의 존재를 외부로부터 숨기고 싶기 때문이다.
리버스 프록시 서버를 사용하는 이유는 여러가지인데 링크 걸어놓은 위키피디아를 참고하기 바란다.

아래 명령을 실행하여 Apache 리버스 프록시 서버를 구성하도록 합니다.
```sh
$ sudo a2enmod proxy
$ sudo a2enmod proxy_http
```

아래 명령어를 실행하고 아래와 같이 내용을 작성합니다.
아래 내용을 작성하게 되면 외부 웹브라우저에서 아래 설정한 URL 주소를 입력하면 Jenkins에 접속할 수 있도록 합니다.
```sh
$ sudo vim /etc/apache2/mods-enabled/proxy.conf
```
![이미지](/files/deploy-war-to-tomcat-jenkins-06.png)

아래 명령어를 실행하여 아래와 같이 내용을 추가합니다.
```sh
$ sudo vim /etc/apache2/sites-enabled/000-default.conf
```
![이미지](/files/deploy-war-to-tomcat-jenkins-07.png)

Apache 웹 서버 관련 설정을 변경한 것이기 때문에 Apache 웹 서버 서비스를 재시작해줘야 한다.

아래 명령어를 실행하여 Apache 웹 서버 서비스를 재시작하도록 한다.
```sh
$ sudo service apache2 restart
```

## 6. 젠킨스(Jenkins) 방화벽 설정하기
Jenkins가 사용할 *8081* 포트번호로 외부에서 접속하도록 하기위해 아래 명령어를 실행하여 방화벽 설정을 한다.
```sh
$ sudo ufw allow 8081/tcp
```

이제 웹브라우저에서 아래와 같이 주소를 입력하여 Jenkins 사이트에 접속이 되는지 확인해본다.<br/>
(192.168.0.67 부분은 본인의 서버 컴퓨터 IP 주소를 입력한다.)
> http://192.168.0.67:8081/jenkins

여기까지 하면 젠킨스 설치가 완료된 것이다.

이제 자동배포를 위한 일련의 젠킨스 설정을 해줘야 한다. 다음 글에서 자세히 설명하겠다.