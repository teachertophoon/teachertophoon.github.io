---
layout: post
title: "[애플리케이션 배포] Ubuntu에 JDK(Java Development Kit) 설치하기"
date: 2018-03-12 23:59:59 +0900
tags: [jdk, ubuntu]
comments: true
---
## 1. JDK(Java Development Kit) 저장소를 추가하기
우분투(Ubuntu)에 JDK 설치파일이 있는 저장소를 추가해줘야 한다. 저장소를 추가해야 JDK를 설치 할 수 있기 때문이다.<br/>
이 글에서는 WebUpd8 팀에서 제공하는 JDK(Java Development Kit) 저장소를 추가한다. 이유는 오라클(Oracle)에서 공식적으로 JDK 설치를 우분투에서 지원하지 않기 때문이다.<br/>
저장소를 추가하기 위해선 아래 명령어를 실행한다.
```sh
$ sudo add-apt-repository ppa:webupd8team/java
```
명령어를 실행 후 아래 화면의 노란색 부분에서 엔터(Enter)키를 입력한다.<br/>
저장소가 추가되면 아래 명령어를 실행하여 패키지(스마트폰으로 치면 앱과 같은 것) 인덱스 정보를 업데이트 한다.
```sh
$ sudo apt-get update
```

![이미지](/files/setup-jdk-to-ubuntu-01.png)

## 2. JDK 설치하기
패키지 인덱스 정보를 업데이트 한 후 아래 명령어를 실행하여 JDK 설치를 시작한다.
```sh
$ sudo apt-get install oracle-java8-installer
```
명령어를 입력하면 아래와 같은 화면이 나타나는데 *OK*를 선택한다.

![이미지](/files/setup-jdk-to-ubuntu-02.png)

JDK를 설치하려면 Oracle Binary Code 라이선스에 동의해야 한다. 아래 화면과 같이 *Yes*를 선택한다.

![이미지](/files/setup-jdk-to-ubuntu-03.png)

JDK 설치가 끝나면 아래 명령어를 아래 화면과 같이 실행하여 설치한 버전이 원하는 버전이 맞는지 확인한다.
```sh
$ java -version
```

![이미지](/files/setup-jdk-to-ubuntu-04.png)

JDK 설치 경로를 확인하기 위해 아래 명령어를 실행한다. 실행하면 아래와 같이 JDK 설치경로가 출력되는데 잘 기억해둔다.<br/>
(여기서는 /usr/bin/java)
```sh
$ which java
```

![이미지](/files/setup-jdk-to-ubuntu-05.png)

## 3. JDK 환경변수 설정
윈도우의 메모장과 같은 프로그램인 vim을 아래 명령어를 실행하여 설치한다.
```sh
$ sudo apt-get install vim
```

윈도우의 환경변수 설정과 같은 우분투의 환경변수 설정파일 수정을 위한 명령어를 아래와 같이 실행한다.
```sh
$ vim ~/.bashrc
```

vim 편집기에서 편집을 하기위해 영문자 i를 눌러 편집모드로 변경한다.

그런 다음 아래 그림과 같은 내용을 *.bashrc* 파일 맨 마지막에 추가해준다.

편집이 끝났으면 *ESC키*를 누른 다음 *콜론(shift + ;)*을 입력하고 *wq*를 입력한 다음 *엔터키*를 누른다.<br/>
(wq 뜻은 Write & Quit 저장하고 나가기)<br/>
이번 기회에 vi 혹은 vim 사용법을 인터넷 검색을 통해 배워보길 바란다.

![이미지](/files/setup-jdk-to-ubuntu-06.png)

아래 명령어를 실행하여 우분투가 변경된 내용을 알아차리도록 만든다.
```sh
$ source ~/.bashrc
```
그리고 입력이 제대로 됐는지 확인하기 위해 아래 명령어를 실행하여 입력한 값과 동일한지 확인한다.
```sh
$ echo $JAVA_HOME
```
여기까지 제대로 됐다면 JDK 설치와 환경변수 설정이 정상적으로 완료된 것이다.

![이미지](/files/setup-jdk-to-ubuntu-07.png)