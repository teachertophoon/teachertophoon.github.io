---
layout: post
title: "[애플리케이션 배포] VirtualBox에 가상 머신 생성하고 우분투(Ubuntu) 설치하기"
date: 2018-03-10 23:59:59 +0900
tags: [virtual-box, ubuntu]
comments: true
---
## 1. VirtualBox를 이용하여 가상 머신 생성하기
#### (1) Virtual Box 설치
[여기](https://www.virtualbox.org/wiki/Downloads)를 통해 설치한다.

#### (2) Virtual Box 설치를 하면 아래와 같은 화면이 출력된다.
이 화면에서 *새로 만들기(N)* 메뉴를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-01.png)

#### (3) 가상 머신 속성 설정하기
아래와 같은 새로운 창이 출력되는데 아래 그림과 같이 설치할 *운영체제 이름(Ubuntu 16)과 종류(Linux), 버전(Ubuntu (64-bit))*을 작성한다.<br/>
메모리 크기는 *2048MB*로 설정한다. (추후 오라클 데이터베이스에서 필요한 메모리 크기가 2GB 이기 때문입니다.)<br/>
그리고 *지금 새 가상 하드 디스크 만들기(C)* 선택 후 *만들기* 버튼 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-02.png)

아래와 같이 파일 크기 항목에 20GB로 설정한다.<br/>
(본인이 원하는 가상컴퓨터에서 사용할 하드디스크 용량 크기를 설정하는 부분)<br/>
그런 다음 만들기 버튼을 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-03.png)

## 2. 우분투(Ubuntu)를 가상 머신에 설치하기
#### (1) 새로 만든 서버 컴퓨터를 아래와 같이 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-04.png)

#### (2) 우분투(Ubuntu) 이미지를 가상 머신에 추가하기
아래와 같이 화면 우측의 *저장소* 항목에 *[광학 드라이브] 비어 있음* 항목을 클릭하면 팝업 메뉴가 표시된다.<br/>
표시된 항목 중 *디스크 이미지 선택...* 항목을 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-05.png)

파일 선택하는 창이 아래와 같이 출력된다.<br/>
[여기](https://www.ubuntu.com/download/server)에서 *Ubuntu Server 16.04.4 LTS*를 내려받는다. 내려받은 iso 파일을 아래 그림처럼 선택한 후 *열기* 버튼을 클릭한다.

그런 다음 Virtual Box 화면에서 가상 머신을 선택한 후 *시작* 버튼을 클릭한다.<br/>
우분투의 하드웨어 최소 요구사항은 [여기](https://help.ubuntu.com/lts/installation-guide/powerpc/ch03s04.html)에서 확인해본다.

![이미지](/files/setup-ubuntu-to-virtualbox-06.png)

#### (3) 우분투(Ubuntu) 설치하기
시작을 눌러 가상 머신 부팅이 완료되면 아래와 같은 화면이 나오는데 설치할 언어를 선택한다. 여기서는 *English*를 선택한다.<br/>
(다른 언어를 선택하면 언어문제로 설치가 원활하게 진행되지 못하는 문제가 발생할 소지가 있기 때문입니다.)

![이미지](/files/setup-ubuntu-to-virtualbox-07.png)

설치 언어를 선택 후 아래와 같은 화면이 출력된다.<br/>
설치하기 위해 *Install Ubuntu Server* 메뉴를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-08.png)

운영체제에서 사용할 기본 언어를 선택하는 화면이 아래와 같이 출력된다.<br/>
역시 마찬가지로 *English*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-09.png)

지역을 선택하는 화면이 아래와 같이 출력된다.<br/>
현재 메뉴에서는 대한민국이 없기 때문에 *United States* 를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-10.png)

아래 화면은 키보드 설정을 일련의 키를 눌러 설정할 것인지 물어보는 화면이다.<br/>
직접 설정을 할 것이기 때문에 *No*를 선택한다. <br/>
(설치를 할땐 천천히 읽어보고 진행하길 바란다. 습관적으로 Yes를 선택하면 낭패보기 십상이다.)

![이미지](/files/setup-ubuntu-to-virtualbox-11.png)

아래화면은 키보드 언어 선택하는 화면이다. 우리는 한국어 키보드를 사용하고 있기 때문에 *Korean*을 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-12.png)

아래 화면이 출력되면 기본키 사용하는 *Korean* 항목을 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-13.png)

컴퓨터의 이름을 지어주는 화면이 아래와 같이 출력된다. 여기서는 *ubuntu*라고 이름을 지어주었다.<br/>
이름을 작성 후 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-14.png)

아래 화면은 root 계정이 아닌 일반 사용자 계정을 생성하는 화면이다.<br/>
생성할 사용자 계정 이름을 여기서는 *ubuntu*로 생성하겠다.<br/>
계정 이름을 작성 후 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-15.png)

생성한 계정의 사용자 이름을 입력하는 화면이 아래와 같이 출력된다. 원하는 사용자 이름(여기서는 *ubuntu*)을 입력 후 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-16.png)

생성한 사용자에 대한 비밀번호를 설정하는 화면이 아래와 같이 출력된다. 여기서는 비밀번호를 *ubuntu*로 입력했다.
입력 후 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-17.png)

아래 화면은 비밀번호를 보안에 취약하게 설정할 때 출력되는 화면이다.<br/>
우리는 실습할 목적이기 때문에 아래와 같이 *Yes*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-18.png)

아래 화면은 우분투의 home 디렉토리(윈도우의 사용자 폴더와 같은 폴더)를 암호화 할 것인지 물어보는 것인데, 암호화 필요가 없으므로 *No*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-19.png)

이번엔 시간 설정 화면이 아래와 같이 출력된다. 자동으로 현재 Time Zone이 Asia/Seoul로 잡혔다.<br/>
맞게 찾았기 때문에 *Yes*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-20.png)

디스크 파티션(디스크 분할) 설정화면이 아래와 같이 출력된다.<br/>
LVM(Logical Volume Manager)을 사용하지 않을 것이기 때문에 첫번째 항목인 *Guided - use entire disk*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-21.png)

파티션 작업을 할 디스크를 아래와 같이 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-22.png)

선택한 디스크에 아래 화면과 같은 파티션으로 설정할 것인데 그대로 진행할지 물어본다. *Yes*를 선택하고 진행한다.

![이미지](/files/setup-ubuntu-to-virtualbox-23.png)

HTTP proxy 관련 부분인데 이 부분도 사용하지 않을 것이기 때문에 값을 입력하지 않고 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-24.png)

보안 관련 업데이트를 자동으로 할 것인지 물어보는 화면인데 자동으로 업데이트를 하지 않을 것이기 때문에 아래와 같이 *No automatic updates*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-25.png)

필요한 소프트웨어 설치하는 화면이 아래와 같이 출력된다. <br/>
기본 구성으로 설치할 것이기 때문에 *standard system utilities*를 선택 후 *Continue*를 선택한다.

![이미지](/files/setup-ubuntu-to-virtualbox-26.png)

GRUB(https://ko.wikipedia.org/wiki/GRUB) 부트로더를 설치를 해야 운영체제가 부팅이 되기 때문에 *Yes*를 선택해서 설치를 한다.

![이미지](/files/setup-ubuntu-to-virtualbox-27.png)

설치가 정상적으로 완료되었다면 아래와 같이 *Finish the installation* 화면이 출력될 것이다.<br/>
설치 미디어(CD-ROM 혹은 iso 파일 등)를 제거하고 *Continue*를 선택하면 된다.<br/>
Virtual Box에서 똑똑하게 iso 파일을 자동으로 해제해주기 때문에 *Continue*만 선택해주면 된다.

![이미지](/files/setup-ubuntu-to-virtualbox-28.png)

서버 컴퓨터가 재부팅되면 아래와 같은 CLI(Command-Line Interface 또는 CUI (Character User Interface)) 화면이 출력된다.
아래와 같이 출력됐다면 정상적으로 우분투 운영체제를 설치한 것이다.

![이미지](/files/setup-ubuntu-to-virtualbox-29.png)