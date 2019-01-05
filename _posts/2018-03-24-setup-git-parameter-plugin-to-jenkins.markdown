---
layout: post
title: "[애플리케이션 배포] Jenkins - Git Tag 기준으로 빌드하기(Git Parameter Plugin)"
date: 2018-03-24 23:59:59 +0900
tags: [jenkins, plugin]
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

1. 아래 그림과 같이 Jenkins 홈 화면에서 *[Jenkins 관리]* 메뉴 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-01.png)

2. 아래 그림과 같이 *[플러그인 관리]* 메뉴 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-02.png)

3. *[설치 가능]* 탭을 선택하여 아래와 같이 *Git Parameter Plug-In* 항목 선택하고 *[지금 다운로드하고 재시작 후 설치하기]* 버튼을 클릭한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-03.png)

4. 플러그인 설치가 끝나면 본인의 *Jenkins 프로젝트 홈 화면*으로 들어가서 아래와 같이 *[구성]* 메뉴를 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-04.png)

5. *[이 빌드는 매개변수가 있습니다]* 항목을 체크하고 *Git Parameter*를 선택한 다음 아래와 같이 내용을 작성한다.
```
Name: tag
Parameter Type: Tag
Branch: */master
```

![이미지](/files/setup-git-parameter-plugin-to-jenkins-05.png)

6. *소스 코드 관리* 영역에서 *Branches to build* 항목의 *Branch Specifier (blank for 'any')* 항목에 아래와 같이 *${tag}*를 입력한다.
*${tag}*에서 tag는 앞에서 Git Parameter의 Name 항목에 입력한 값이다.
설정이 끝나면 아래쪽의 *[저장]* 버튼을 눌러 설정을 저장한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-06.png)

7. 본인의 *Jenkins 프로젝트 홈 화면*으로 돌아가면 아래와 같이 Build 메뉴가 Build with Parameters 메뉴로 변경된 것을 확인할 수 있다. *Build with Parameters* 메뉴를 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-07.png)

8. Git을 이용하여 Tag를 생성했다면 아래와 같이 생성한 Tag 목록이 나타난다.
원하는 Tag를 선택하여 *빌드하기* 버튼을 선택하면 해당 Tag 기준으로 빌드가 실행된다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-08.png)

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