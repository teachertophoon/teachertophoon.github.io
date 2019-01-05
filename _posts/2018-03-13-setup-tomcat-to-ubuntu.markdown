---
layout: post
title: "[애플리케이션 배포] Apache Tomcat 설치하기"
date: 2018-03-13 23:59:59 +0900
tags: [tomcat, ubuntu]
comments: true
---
이 글과 관련된 포스트 목록입니다. (학습 순서대로 정렬)
1. [[애플리케이션 배포] VirtualBox에 가상 머신 생성하고 우분투(Ubuntu) 설치하기](https://blog.tophoon.com/2018/03/10/setup-ubuntu-to-virtualbox.html)
2. [[애플리케이션 배포] PuTTY를 이용하여 내 컴퓨터에서 가상 머신(서버)로 원격 접속하기](https://blog.tophoon.com/2018/03/11/connecting-local-remote-using-putty.html)
3. [[애플리케이션 배포] 우분투(Ubuntu)에 JDK(Java Development Kit) 설치하기](https://blog.tophoon.com/2018/03/12/setup-jdk-to-ubuntu.html)
4. [[애플리케이션 배포] Apache Tomcat 설치하기](https://blog.tophoon.com/2018/03/13/setup-tomcat-to-ubuntu.html)
5. [[애플리케이션 배포] Oracle Database 설치하기 (11g XE 버전 설치하기)](https://blog.tophoon.com/2018/03/14/setup-oracle-to-ubuntu.html)
6. [[애플리케이션 배포] 우분투(Ubuntu)에 MySQL 설치하기](https://blog.tophoon.com/2018/03/20/setup-mysql-to-ubuntu.html)
7. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 수동으로 배포](https://blog.tophoon.com/2018/03/21/deploy-war-to-tomcat-manually.html)
8. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (1)](https://blog.tophoon.com/2018/03/22/deploy-war-to-tomcat-jenkins.html)
9. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (2)](https://blog.tophoon.com/2018/03/23/deploy-war-to-tomcat-jenkins.html)
10. [[애플리케이션 배포] Jenkins - Git Tag 기준으로 빌드하기(Git Parameter Plugin)](https://blog.tophoon.com/2018/03/24/setup-git-parameter-plugin-to-jenkins.html)

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

이 글과 관련된 포스트 목록입니다. (학습 순서대로 정렬)
1. [[애플리케이션 배포] VirtualBox에 가상 머신 생성하고 우분투(Ubuntu) 설치하기](https://blog.tophoon.com/2018/03/10/setup-ubuntu-to-virtualbox.html)
2. [[애플리케이션 배포] PuTTY를 이용하여 내 컴퓨터에서 가상 머신(서버)로 원격 접속하기](https://blog.tophoon.com/2018/03/11/connecting-local-remote-using-putty.html)
3. [[애플리케이션 배포] 우분투(Ubuntu)에 JDK(Java Development Kit) 설치하기](https://blog.tophoon.com/2018/03/12/setup-jdk-to-ubuntu.html)
4. [[애플리케이션 배포] Apache Tomcat 설치하기](https://blog.tophoon.com/2018/03/13/setup-tomcat-to-ubuntu.html)
5. [[애플리케이션 배포] Oracle Database 설치하기 (11g XE 버전 설치하기)](https://blog.tophoon.com/2018/03/14/setup-oracle-to-ubuntu.html)
6. [[애플리케이션 배포] 우분투(Ubuntu)에 MySQL 설치하기](https://blog.tophoon.com/2018/03/20/setup-mysql-to-ubuntu.html)
7. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 수동으로 배포](https://blog.tophoon.com/2018/03/21/deploy-war-to-tomcat-manually.html)
8. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (1)](https://blog.tophoon.com/2018/03/22/deploy-war-to-tomcat-jenkins.html)
9. [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (2)](https://blog.tophoon.com/2018/03/23/deploy-war-to-tomcat-jenkins.html)
10. [[애플리케이션 배포] Jenkins - Git Tag 기준으로 빌드하기(Git Parameter Plugin)](https://blog.tophoon.com/2018/03/24/setup-git-parameter-plugin-to-jenkins.html)