---
layout: post
title: "GitHub Pages와 Jekyll Framework를 이용하여 나만의 블로그 만들기 (2) - Jekyll로 내 컴퓨터에 블로그 사이트 생성하기"
date: 2019-01-27 14:00:00 +0900
tags: [github-pages, jekyll]
comments: true
---
{%- include package-jekyll.markdown -%}

이 글은 아래 사이트를 참고하여 작성한 글입니다.
- [Quickstart](https://jekyllrb.com/docs)

이전 글에서 Jekyll을 사용하기 위한 Ruby 언어 기본 설치법과 Jekyll 프레임워크 설치법을 알아보았습니다.

이번 글에서는 Jekyll 프레임워크를 이용하여 로컬(본인 컴퓨터)에 블로그 사이트를 생성하고 웹브라우저를 이용하여 생성한 블로그에 접속하는 방법을 알아보겠습니다. [이전글](https://blog.tophoon.com/2019/01/12/setup-ruby-for-jekyll.html) 내용을 따라 하셔야 이번 글 내용을 따라하실 수 있습니다.

## 1. 새로운 지킬(Jekyll) 사이트 생성하기
우선 명령 프롬프트(터미널)를 실행시킨다.

예) 윈도우 일 경우 `윈도우 키 + R` 누른 후 입력 창에 `cmd` 입력하고 확인

아래와 같이 명령어를 이용하여 블로그 사이트를 생성할 최상위 디렉토리로 이동한다. 여기서는 `C:\` 경로 아래에 블로그 사이트를 생성할 것이다.
```sh
C:\Users\[사용자명]> cd \
C:\>

```

새로 만들 블로그 사이트 이름이 `myblog` 이라면 아래와 같이 명령어를 실행하여 Jekyll 기본 사이트를 생성한다.
```sh
C:\> jekyll new myblog
```

문제가 없다면 `New jekyll site installed in C:/myblog.` 메시지가 출력될 것이다.

아래와 같이 명령어를 실행하여 기본 사이트가 생성된 폴더로 이동한다.
```sh
C:\> cd myblog
C:\myblog>
```

## 2. 사이트 접속을 위한 서버 실행하기
아래와 같이 번들러(Bundler) 명령어 중 `bundle exec` 명령어를 사용하여 `jekyll serve`라는 명령어를 bundle 컨텍스트 내에서 실행시킨다.
명령어에 대한 설명은 [여기](https://bundler.io/man/bundle-exec.1.html)를 참고하길 바란다.
```sh
C:\myblog> bundle exec jekyll serve
```

실행을 시키고 아래와 같은 메시지가 출력된다면 정상적으로 실행된 것이다. 우리가 실행시킨 것은 블로그 사이트에 접속할 수 있는 서버를 실행시킨 것이다.
```sh
Configuration file: C:/myblog/_config.yml
            Source: C:/myblog
       Destination: C:/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.957 seconds.
 Auto-regeneration: enabled for 'C:/myblog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

메시지를 읽어보면 서버주소가 `http://127.0.0.1:4000`이고 서버를 중지시키고 싶다면 `Ctrl키 + C`를 눌러 중지시킬 수 있다고 한다.

## 3. 실행한 서버(사이트)에 접속하기
그럼 우리가 사용하는 웹브라우저(예: 크롬)를 실행하여 서버주소를 입력하여 접속해보자.
그럼 아래와 같은 사이트가 출력될 것이다.

![이미지](/files/jekyll-quick-start-01.png)

서버를 중지시키려면 우리가 열었던 명령 프롬프트 창으로 돌아가서 `Ctrl키 + C`를 누른다. `일괄 작업을 끝내시겠습니까 (Y/N)?`라는 메시지가 출력될텐데, `y`를 입력한 다음 엔터키를 누른다. 그러면 앞에서 입력했던 서버주소로 접속이 되지 않을 것이다. (당연히 서버를 껐기 때문에 접속이 안된다.)

위와 같이 출력된 사이트는 Jekyll이 가지고 있는 기본 틀을 이용하여 생성된 것이다.
앞으로 우린 이 사이트에 글도 쓰고 자기가 원하는대로 꾸밀 수도 있다.

그리고 지금은 내가 만든 블로그에 내 컴퓨터에서만 접속할 수 있는 상태이다.
다른 사람들도 접속 가능하도록 [웹 호스팅 서비스](https://en.wikipedia.org/wiki/Web_hosting_service)를 이용해야 한다.
수많은 웹 호스팅 서비스 중 `GitHub Pages`라는 웹 호스팅 서비스가 있다. 무료이고 GitHub 저장소를 이용하기 때문에 개발자에게 친숙한 서비스이다.
그래서 여기서는 GitHub Pages를 이용하도록 하겠다.

## 맺음말
이 글에서는 Jekyll 프레임워크를 이용하여 로컬(본인 컴퓨터)에 블로그 사이트를 생성하고 웹브라우저를 이용하여 생성한 블로그에 접속하는 방법을 알아보았습니다.

다음 글에서는 블로그에 글을 작성하는 방법과 다른 사람들도 접속 가능하도록 웹 호스팅 서비스(GitHub Pages)를 이용하는 방법에 대해 설명하도록 하겠습니다.

{%- include package-jekyll.markdown -%}