---
layout: post
title: "[애플리케이션 배포] 웹 애플리케이션 Tomcat 서버에 배포하기 - 수동으로 배포"
date: 2018-03-21 23:59:59 +0900
tags: [war, deploy, tomcat, manually]
comments: true
---
이번 글은 웹 애플리케이션을 Tomcat 서버에 수동으로 배포(Deploy)하는 방법을 설명한 글이다. 배포한다는 말은 보통 *'작업한 내용을 서버에 올린다.'*라는 뜻으로 사용된다.

나중에 젠킨스(Jenkins)를 이용해서 자동으로 배포하기 전에, 배포되는 원리를 이해하기 위해서 한번 수동으로 배포하는 방법을 따라해보면 좋겠다.

## 1. [WAR 파일](https://en.wikipedia.org/wiki/WAR_(file_format)) 생성하기
WAR 파일은 간단히 설명드리면 웹 애플리케이션 보관소라는 뜻으로 우리가 만든 웹 애플리케이션을 하나의 파일로 묶어놓은 것이라고 이해하면 되겠다.

이 WAR 파일을 Tomcat 서버의 특정 폴더에 복사-붙여넣기만 하면 수동으로 배포하는 것은 끝나게 된다.

아래 화면과 같이 배포할 프로젝트를 마우스 오른쪽 버튼으로 선택하여 *Export 메뉴*를 선택한다.

![이미지](/files/deploy-war-to-tomcat-manually-01.png)

아래 화면과 같이 *WAR file* 항목 선택 후 *Next 버튼* 선택한다.

![이미지](/files/deploy-war-to-tomcat-manually-02.png)

아래 화면과 같이 *WAR 파일 생성 할 디렉토리를 Destination 항목에서 지정*한 후 *Finish 버튼*을 선택한다.
그러면 해당 디렉토리에 WAR 파일이 생성된 것을 확인할 수 있다.

![이미지](/files/deploy-war-to-tomcat-manually-03.png)

## 2. 생성한 WAR 파일을 서버로 전송하기
생성한 WAR 파일을 서버로 전송하기 위해 FTP 프로그램을 설치한다. [여기](https://filezilla-project.org)로 이동하여 'FileZilla Client'를 내려받아 설치한다.

아래 화면과 같이 FileZilla 프로그램을 실행하여 *파일 메뉴의 사이트 관리자* 메뉴를 선택한다.
![이미지](/files/deploy-war-to-tomcat-manually-04.png)

*새 사이트 버튼*을 선택하고 아래화면과 같이 내용을 작성한다.

호스트 항목에는 서버의 IP 주소를 입력하고 프로토콜은 SFTP를 선택한다. 사용자 항목과 비밀번호 항목은 서버 컴퓨터 부팅 시 사용하는 사용자 아이디와 비밀번호를 입력한다. 그런 다음 *연결 버튼*을 선택한다.

![이미지](/files/deploy-war-to-tomcat-manually-05.png)

아래 화면과 같이 */var/lib/tomcat8/webapps* 경로로 이동하여 ROOT 폴더를 제거한다.

제거가 되지 않는다면 PuTTY로 서버에 접속한 다음, 아래 명령어를 이용하여 직접 삭제한다.
```sh
$ cd /var/lib/tomcat8/webapps
$ sudo rm -rf ROOT.war
$ sudo rm -rf ROOT
```

![이미지](/files/deploy-war-to-tomcat-manually-06.png)

FileZilla에서 로컬 컴퓨터에 있는 WAR 파일을 전송하기 위해서는 PuTTY로 서버에 접속한 다음 아래 명령을 실행하여 권한을 변경해줘야 한다.
```sh
$ sudo chmod 777 /var/lib/tomcat8/webapps
```

FileZilla로 WAR 파일을 */var/lib/tomcat8/webapps*로 복사한 뒤, 아래 명령을 실행하여 webapps의 원래 권한으로 변경한다.
```sh
$ sudo chmod 775 /var/lib/tomcat8/webapps
```

WAR 파일 수동배포가 완료되었다면 아래 명령어를 실행하여 Tomcat 서비스를 재시작한다.
```sh
$ systemctl restart tomcat8
```

여기까지 하면 수동으로 배포가 끝난 것이다. 배포를 했으니 실제 사이트에 접속하여 문제가 없는지 확인하도록 한다.

그런데 수동으로 배포하면 아래와 같은 문제점이 발생한다.
- WAR 파일을 직접 만들고 서버로 복사해줘야 해서 번거롭다.
- 수작업을 하면 실수가 발생할 소지가 있다.

그래서 다음 글에서는 자동으로 배포하는 방법을 알아보도록 하겠다.

## 번외: 문제 발생 시 대처방법
만약 아래와 같은 화면이 출력된다면 multi-part 관련 Tomcat 설정을 아래 명령어를 이용하여 변경해줘야 한다.

![이미지](/files/deploy-war-to-tomcat-manually-07.png)

```sh
$ cd /var/lib/tomcat8/conf
$ sudo vim context.xml
```

위 명령어를 실행 후 아래와 같은 부분을 찾는다.
```sh
...
<Context>
...
```

Context 요소(Element)를 찾은 다음 아래와 같이 수정해준다. 아래와 같이 설정하면 multi-part 관련 전송을 허용해준다는 뜻이다.
```sh
...
<Context allowCasualMultipartParsing="true" path="/">
...
```