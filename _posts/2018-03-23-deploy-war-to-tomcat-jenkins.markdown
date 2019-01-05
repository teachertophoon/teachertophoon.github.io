---
layout: post
title: "[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (2)"
date: 2018-03-23 23:59:59 +0900
tags: [war, deploy, tomcat, jenkins]
comments: true
---
{%- include package-how-to-deploy-an-application.markdown -%}

*이전 글 [[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 젠킨스(Jenkins) (1)](https://blog.tophoon.com/2018/03/22/deploy-war-to-tomcat-jenkins.html)를 먼저 읽으시고 진행하시기 바랍니다.*

## 1. 젠킨스(Jenkins) 초기설정하기
정상적으로 Jenkins에 접속이 됐다면 아래와 같은 Jenkins 사이트 화면이 나타날 것이다.
화면에 나타난대로 */var/lib/jenkins/secrets/initialAdminPassword* 파일을 vim으로 열어 비밀번호를 확인 한 뒤 *initialAdminPassword* 파일 내용을 복사하여 *Adiministrator password* 항목에 입력한다. 그런 다음 *Continue*를 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-08.png)

아래 그림과 같이 왼쪽 항목인 Install suggested plugins를 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-09.png)

Jenkins에 접속할 계정을 등록하는 화면이 아래와 같이 출력된다. 각 항목에 맞는 내용을 채운 다음 *Save and Finish*를 선택하여 다음 단계로 진행한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-10.png)

Jenkins가 준비되었다고 한다. *Start using Jenkins*를 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-11.png)

## 2. 빌드 도구(Build Tool) 경로 설정하기
좌측메뉴의 *Jenkins 관리* 메뉴를 선택하고 우측의 필터 항목에 *Deploy to container Plugin*을 검색한 다음 체크하고 *지금 다운로드하고 재시작 후 설치하기*를 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-12.png)

Jenkins가 재시작되면 *Jenkins 관리* 메뉴를 선택하고 *Global Tool Configuration* 메뉴를 선택한다.
선택하면 아래와 같은 화면이 나타나는데 우리가 서버에 설치했던 JDK 경로를 아래와 같이 작성한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-13.png)

그 다음 우리가 Github에서 받은 Maven 프로젝트를 빌드하기 위해서는 Maven을 설치해야 하는데 Jenkins 자체에서 Maven을 설치할 수 있다. 아래와 같이 작성하면 Jenkins에서 알아서 Maven을 설치해준다. 모두 다 설정을 했다면 *Save* 버튼을 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-14.png)

## 3. 젠킨스(Jenkins) 계정 사용을 위한 환경변수 설정 및 패키지 설치하기
아래 명령어를 실행하고 아래와 같이 내용을 작성한다.
```sh
$ sudo vim /var/lib/tomcat8/conf/tomcat-users.xml
```
![이미지](/files/deploy-war-to-tomcat-jenkins-15.png)

아래 명령어를 실행하여 설치한다.
```sh
$ sudo apt-get install tomcat8-admin
```

Jenkins에서 생성한 WAR파일을 webapps로 복사하게 되고, 톰켓(Tomcat)은 이 WAR 파일을 자동으로 풀게 된다.
이때, webapps에 ROOT 폴더가 존재하면 파일이 풀어지지 않기 때문에 아래 명령어를 실행하여 해당 폴더 내용을 비워준다.
```sh
$ sudo rm -rf /var/lib/tomcat8/webapps/*
```

## 4. 젠킨스(Jenkins) 프로젝트 추가하기
이제 자동 배포를 위해 새로운 item 메뉴를 선택하여 자동 배포 할 프로젝트를 등록한다.
아래 화면과 같이 등록할 프로젝트명을 작성하고 *Freestyle project* 메뉴를 선택한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-16.png)

Jenkins는 Github에 있는 프로젝트 파일을 내려받은 다음 프로젝트의 pom.xml 파일을 읽어 빌드를 한다. 매번 빌드 할 때마다 빌드 내용을 보관을 하는데, 아래와 같이 작성을 하면 프로젝트 파일들을 최대 10개까지만 보관하고 나머지 오래된 빌드는 삭제한다. 아래와 같이 설정하지 않고 사용한다면 서버에 용량이 부족해질 수도 있기 때문에 아래와 같이 설정을 하였다.

![이미지](/files/deploy-war-to-tomcat-jenkins-17.png)

아래 그림과 같이 소스 코드 관리 항목에서 Git을 선택하고 프로젝트의 저장소 경로(Repository URL)을 입력하고 Credentials 항목의 Add 버튼을 눌러 Github 아이디와 비밀번호를 등록해준다. 아이디와 비밀번호를 입력하는 이유는 Jenkins가 자동으로 Github에 접속해서 프로젝트 파일을 내려받기 위함이다.

![이미지](/files/deploy-war-to-tomcat-jenkins-18.png)

*Credentials* 항목의 *Add* 버튼을 누르면 아래와 같은 화면이 출력되는데 아래와 같이 입력을 하면 된다.<br/>
(아래는 예시이므로 본인의 사용자명과 비밀번호를 입력하길 바란다.)

입력이 다 됐다면 하단의 *Add* 버튼을 누른다.

![이미지](/files/deploy-war-to-tomcat-jenkins-19.png)

빌드 환경 항목에서 *Delete workspace before build starts* 항목에 아래와 같이 체크해준다. 빌드 전에 이전에 내려받았던 프로젝트 파일들을 삭제하라는 뜻이다. 
이렇게 하는 이유는 이전 빌드에 사용된 파일이 남아 현재 빌드에 영향을 끼치지 않게 하기 위함이다.

![이미지](/files/deploy-war-to-tomcat-jenkins-20.png)

Build 항목에서 우리가 Jenkins에서 설치한 Maven을 선택하고 아래와 같이 작성해준다. POM 항목은 실제 프로젝트 경로에서 pom.xml 파일이 위치한 경로를 입력한다. Goals 항목은 아래와 같이 *clean package*로 작성한다.<br/>
(*clean*은 이전 빌드에서 생성된 모든 파일을 삭제한다는 의미, *package*는 컴파일 된 코드를 가져 와서 WAR과 같은 배포 가능한 형식으로 패키지화 한다는 의미)

Goals 항목에 작성하는 내용에 대한 설명은 [여기](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)를 참고하기 바란다.

![이미지](/files/deploy-war-to-tomcat-jenkins-21.png)

빌드 후 조치 항목에서는 *Deploy war/ear to a container* 항목을 선택해서 아래와 같은 내용을 작성한다.
이 부분을 작성하면 Jenkins가 빌드를 끝마치고 WAR 파일이 생성되는데 생성한 WAR 파일을 Tomcat 컨테이너 폴더로 복사해준다.
그 다음 Containers 항목에 Tomcat 사용자에 대한 내용을 등록해줘야 하는데 *큰 제목 3번*에서 등록했던 *user1*에 대한 정보를 추가해준다. *Add* 버튼을 눌러 추가해준다.

![이미지](/files/deploy-war-to-tomcat-jenkins-22.png)

*Add* 버튼을 누르면 아래와 같은 화면이 출력된다. 이전에 아이디 *user1* 비밀번호를 *system*으로 등록해 놓은 사용자 정보를 아래와 같이 입력한다. 입력이 끝나면 *Add* 버튼을 눌러 빠져나온다.

![이미지](/files/deploy-war-to-tomcat-jenkins-23.png)

다시 빌드 후 조치 항목으로 돌아오면 아래와 같은 화면이 출력된다. Tomcat URL를 입력한다. (여기서는 8082 포트번호를 사용할 경우의 예시화면이다. Tomcat 설정파일에서 사용했던 포트번호를 사용하길 바란다.)
그리고 *Delete workspace when build is done* 항목을 추가하여 빌드가 완료되면 Jenkins의 workspace를 삭제하도록 하여 서버 용량이 부족해지지 않도록 한다.

![이미지](/files/deploy-war-to-tomcat-jenkins-24.png)

이제 서버를 재부팅하고 프로젝트 사이트에 접속해본다. 프로젝트 첫 화면이 출력된다면 자동 배포하는 환경을 제대로 구축한 것이다.

{%- include package-how-to-deploy-an-application.markdown -%}