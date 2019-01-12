---
layout: post
title: "GitHub Pages와 Jekyll Framework를 이용하여 나만의 블로그 만들기 (1) - Ruby 언어와 Jekyll 설치하기"
date: 2019-01-12 21:30:00 +0900
tags: [github-pages, jekyll, ruby, gem]
comments: true
---
이 글은 아래 사이트를 참고하여 작성한 글입니다.
- [Jekyll on Windows](https://jekyllrb.com/docs/installation/windows/)

## 루비(Ruby) 언어 설치하기
루비 언어는 자바(Java)와 마찬가지로 프로그래밍 언어 중 하나이다.

루비 언어를 설치하는 이유는 앞으로 우리가 사용할 Jekyll(지킬) 프레임워크(Framework)가 [Ruby Gem](https://en.wikipedia.org/wiki/RubyGems)으로 작성되었기 때문이다.

여기서는 Windows 운영체제 기준으로 루비 언어를 설치하는 방법을 설명하겠다.

#### 1. RubyInstaller 설치파일 내려받기
[RubyInstaller](https://rubyinstaller.org) 사이트에 접속하면 아래와 같은 화면이 나타난다. `Download` 버튼을 누른다.

![이미지](/files/setup-ruby-for-jekyll-01.png)

그러면 아래와 같은 화면이 나타난다. 우측 설명을 읽어보면 아래 내용과 같다.
> 만약 어떤 버전을 설치해야 할지 모르거나 Ruby를 시작해보려는 경우에는 Ruby+Devkit 2.5.X 버전을 설치하는 것을 추천한다.

추천내용에 따라 아래 화면에 빨간색 표시한 링크를 클릭한다.<br/>
(루비 언어 버전은 자주 업데이트 되므로 추천하는 버전은 다를 수 있다.)

![이미지](/files/setup-ruby-for-jekyll-02.png)

#### 2. Ruby 설치하기
내려받은 `RubyInstaller` 파일을 실행하면 아래와 같은 라이센스 동의 화면이 나타난다. 무료이므로 부담가지지 말고 `Next`를 선택한다.

![이미지](/files/setup-ruby-for-jekyll-03.png)

아래 화면과 동일하게 설정하고 `Install`을 선택한다.

만약 다른 경로에 설치하고 싶다면 `Browse...`를 눌러 다른 경로로 설정한다.

![이미지](/files/setup-ruby-for-jekyll-04.png)

마찬가지로 아래 화면과 동일하게 설정한 뒤 `Next`를 선택한다.

![이미지](/files/setup-ruby-for-jekyll-05.png)

그러면 아래와 같이 설치가 완료됐다는 창이 출력된다. 체크박스 하나가 있는데 체크하고 `Finish` 버튼을 선택한다.

![이미지](/files/setup-ruby-for-jekyll-06.png)

#### 3. MSYS2 설치하기
이전 화면에서 체크박스를 제대로 체크했다면 아래와 같은 화면이 나타날 것이다.
키보드의 `Enter` 키를 입력하여 모두 설치하도록 한다.

![이미지](/files/setup-ruby-for-jekyll-07.png)

그러면 MSYS2 설치를 진행하게 된다. 설치가 끝나면 아래와 같은 화면이 나타나는데 `Enter`키를 눌러 설치를 종료한다.

![이미지](/files/setup-ruby-for-jekyll-08.png)

#### 4. Jekyll Framework 설치하기
이제 루비 언어를 설치했기 때문에 `명령 프롬프트` 창에서 `gem`이라는 명령어를 사용할 수 있다. `명령 프롬프트`창을 열어서 아래와 같이 명령어를 실행한다.
```sh
> gem install jekyll bundler
```

실행을 하고 기다리면 Jekyll 프레임워크 설치가 끝나게 된다.

설치가 제대로 됐는지 확인하기 위해 `명령 프롬프트` 창에서 아래 명령어를 실행한다.
```sh
> jekyll -v
```

위 명령어는 현재 설치된 Jekyll의 버전을 확인하는 명령어이다. `jekyll`이라는 문구와 버전이 출력됐다면 정상적으로 Jekyll 프레임워크 설치가 끝나게 된 것이다.

## 맺음말
이 글에서는 Ruby 언어 기본 설치법과 Jekyll 프레임워크 설치법을 알아보았다.

다음 글에서는 Jekyll 프레임워크를 이용하여 로컬(본인 컴퓨터)에 블로그 사이트를 생성하고 웹브라우저를 이용하여 블로그에 접속하는 방법을 설명하도록 하겠다.