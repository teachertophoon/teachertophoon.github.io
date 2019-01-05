---
layout: post
title: "[애플리케이션 배포] PuTTY를 이용하여 내 컴퓨터에서 가상 머신(서버)로 원격 접속하기"
date: 2018-03-11 23:59:59 +0900
tags: [putty, firewall, ip-address, port, ubuntu, ssh]
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

만약 여러분의 회사 혹은 개인서버가 미국에 설치되어 있다고 가정해보자. 서버를 사용하기 위해 비행기를 타고 미국까지 가서 서버를 사용하진 않을 것이다.<br/>
그럼 대한민국에 있는 내 컴퓨터로 미국에 있는 서버에 접속하는 방법이 없을까? 방법이 있다.

## 1. 서버에 주소(IP 주소)를 할당하자!
우리가 친구 집을 찾아갈 수 있는 이유는 집에 주소가 할당되어 있기 때문이다.<br/>
우선 서버(가상 머신)에 독립적인 IP 주소를 할당해 주어야 원격접속이 가능하기 때문에 Virtual Box 설정을 변경해준다.<br/>
아래 화면과 같이 설정을 변경할 서버를 선택 후 *설정(S)* 버튼을 클릭한다.

![이미지](/files/connecting-local-remote-using-putty-01.png)

설정버튼을 누르면 아래와 같은 창이 출력된다. 좌측 메뉴에서 *네트워크*를 선택 후, *어댑터 1*탭을 선택한다.
그런 다음 아래 화면과 같이 *브리지 어댑터(혹은 어댑터에 브리지)*를 선택하고 이름 항목은 *본인의 컴퓨터 랜카드 이름*을 선택한다.
그리고 나서 *확인 버튼*을 누른다.

![이미지](/files/connecting-local-remote-using-putty-02.png)

서버 컴퓨터를 부팅한 후 아래 화면과 같이 *ifconfig* 명령어를 입력하여 서버의 IP 주소를 확인한다.

![이미지](/files/connecting-local-remote-using-putty-03.png)

명령어를 입력하면 아래 화면과 같이 출력이 되는데 노란색으로 표시한 부분을 보면 본인의 서버 IP 주소를 확인할 수 있다.<br/>
*이 IP 주소를 기억해 두길 바란다.*

![이미지](/files/connecting-local-remote-using-putty-04.png)

## 2. 외부 접속을 허용하게 하자!
낯선 사람들의 침입을 기본적으로 막고, 내가 허락한 사람들만 들어올 수 있도록 만들어야 한다.<br/>
우분투(Ubuntu)는 기본적으로는 외부 접속이 막혀있는 상태이다. 외부 접속을 허용하기 위해 서버에 SSH를 설치한다.<br/>
아래 화면과 같이 아래 명령을 실행한다.
```sh
$ sudo apt-get install ssh
```
만약 설치가 안된다면 아래와 같이 명령을 실행하길 바란다.
```sh
$ sudo apt-get update
$ sudo apt-get install ssh
```

![이미지](/files/connecting-local-remote-using-putty-05.png)

외부 접속을 허용하기 위해 방화벽을 설정해줘야 한다.
아래 화면과 같이 아래 명령을 이용하여 Ubuntu Fire Wall을 활성화시켜준다.
```sh
$ sudo ufw enable
```

![이미지](/files/connecting-local-remote-using-putty-06.png)

나중에 설치할 PuTTY 프로그램이 서버에 접속하기 위해선 22번 포트(Port)를 열어줘야 한다.<br/>
포트는 간단히 설명드리면 아파트 동은 IP 주소이고 호수는 포트번호라고 생각하면 됩니다.<br/>
22번 포트를 이용해서 PuTTY 프로그램이 서버 컴퓨터로 접속할 수 있도록 아래 화면과 같이 아래 명령어를 이용하여 포트를 열어준다.
```sh
$ sudo ufw allow 22/tcp
```

![이미지](/files/connecting-local-remote-using-putty-07.png)

방금 22번 포트가 정상적으로 열렸는지 확인하기 위해 아래 명령을 이용하여 확인해본다.
```sh
$ sudo ufw status
```
아래와 같은 화면이 출력되면 정상적으로 설정 된 것이다.<br/>
(Action 항목에 ALLOW 값이면 접속 허용이라는 뜻)

![이미지](/files/connecting-local-remote-using-putty-08.png)

## 3. 서버에 접속할 도구(PuTTY)를 설치하자!
멀리있는 사람과 이야기하고 싶을 때 스마트폰을 이용하듯이 원격 서버에 접속하고 싶을 때는 PuTTY라는 터미널 에뮬레이터를 이용한다.
물론 PuTTY 말고도 다양한 터미널 에뮬레이터가 있지만 대표적으로 많이 사용하고 무료인 PuTTY를 설치하자.<br/>
[여기](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)에 접속하여 PuTTY 설치파일을 내려받는다.<br/>
*Alternative binary files* 항목에서 putty.exe 파일을 다운로드 한다.

다운로드한 putty.exe 파일을 실행하면 아래와 같은 화면이 출력된다.<br/>
*Host Name*에 이전 단계에서 확인했던 서버 IP 주소를 입력하고 *Port번호는 22번*을 입력한다.
이 설정을 저장하기 위해 Saved Sessions 항목에 저장하고 싶은 이름을 입력하고 *Save 버튼*을 누른다.
그 다음 저장한 이름을 더블클릭하면 서버로 원격 접속할 수 있다.

![이미지](/files/connecting-local-remote-using-putty-09.png)

처음 서버 컴퓨터에 원격 접속하면 PuTTY에서 보안 경고창을 아래 화면과 같이 출력한다.<br/>
신뢰하지 않으면 아니요 버튼을 누르라고 하는데, 우리는 신뢰하는 서버이니까 *예* 버튼을 누른다.

![이미지](/files/connecting-local-remote-using-putty-10.png)

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