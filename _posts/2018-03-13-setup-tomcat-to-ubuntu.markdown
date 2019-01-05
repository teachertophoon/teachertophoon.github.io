---
layout: post
title: "[애플리케이션 배포] Apache Tomcat 설치하기"
date: 2018-03-13 23:59:59 +0900
tags: [tomcat, ubuntu]
comments: true
---
{%- include package-how-to-deploy-an-application.markdown -%}

## 1. 패키지 인덱스 정보 업데이트 하기
아래 명령어를 실행하여 패키지 인덱스 정보를 업데이트 한다.
```sh
$ sudo apt-get update
```

## 2. 설치된 패키지 업그레이드 하기
아래 명령어를 실행하여 설치된 패키지를 업그레이드 한다.
```sh
$ sudo apt-get upgrade
```

## 3. Tomcat 설치하기
아래 명령어를 실행하여 Tomcat을 설치한다.
```sh
$ sudo apt-get install tomcat8
```

## 4. 환경변수 설정하기
Tomcat 설치 후 Tomcat에게 JDK 설치 경로를 알려주기 위해 아래 명령어를 실행하여 편집한다.
```sh
$ sudo vim /etc/init.d/tomcat8
```
명령어를 실행하면 아래와 같은 화면이 출력된다. *:set number* 를 실행하여 줄 번호를 출력하게 한다.<br/>
81번째 줄로 이동하여 *JDK_DIRS="/usr/lib/jvm/java-8-oracle"*로 변경한다.
*:wq*를 입력하여 파일을 저장하고 나온다.

![이미지](/files/setup-tomcat-to-ubuntu-01.png)

아래 명령어를 입력한다.
```sh
$ systemctl daemon-reload
```

Tomcat 설정 파일 내용을 변경했으므로 변경사항을 적용하기 위해 아래 명령어를 실행하여 Tomcat 서비스를 재실행한다.
```sh
$ systemctl start tomcat8
```
명령어를 실행하면 아래 화면과 같이 비밀번호를 입력하라고 나오는데 우분투 접속 시 사용하는 비밀번호를 입력한다. 정상적으로 입력하면 Tomcat 서비스가 재실행된다.

![이미지](/files/setup-tomcat-to-ubuntu-02.png)

아래 명령어를 실행하여 Tomcat의 conf 폴더로 이동한다.
```sh
$ cd /var/lib/tomcat8/conf
```

아래 명령어를 실행하여 아래내용과 같이 편집한다.
```sh
$ sudo vim server.xml
```
port : 원하는 포트번호 (예: *8080*)<br/>
URIEncoding : URI 인코딩 방식 (예: *UTF-8*)<br/>
maxPostSize : Post 허용 크기 (예를 들면 *1024000000*면 1GB, *0*보다 작은 값은 무제한)<br/>
편집을 끝냈으면 *:wq*를 입력하여 저장하고 나온다.

![이미지](/files/setup-tomcat-to-ubuntu-03.png)

## 5. 방화벽 설정하기
방금 설정한 포트번호로 외부에서 접속을 허용하기 위해 아래와 같이 명령어를 실행한다.<br/>
(포트번호가 8080일 경우)
```sh
$ sudo ufw allow 8080/tcp
```
서버 IP 주소와 포트번호를 웹브라우저 주소창에 입력해서 접속해본다.<br/>
(예: http://192.168.0.67:8080)<br/>
아래와 같은 화면이 출력된다면 Tomcat 설치가 정상적으로 완료된 것이다.

![이미지](/files/setup-tomcat-to-ubuntu-04.png)

{%- include package-how-to-deploy-an-application.markdown -%}