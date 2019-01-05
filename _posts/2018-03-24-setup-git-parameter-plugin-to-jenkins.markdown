---
layout: post
title: "[애플리케이션 배포] Jenkins - Git Tag 기준으로 빌드하기(Git Parameter Plugin)"
date: 2018-03-24 23:59:59 +0900
tags: [jenkins, plugin]
comments: true
---
#
{%- include package-how-to-deploy-an-application.markdown -%}
#

(1) 아래 그림과 같이 Jenkins 홈 화면에서 *[Jenkins 관리]* 메뉴 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-01.png)

(2) 아래 그림과 같이 *[플러그인 관리]* 메뉴 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-02.png)

(3) *[설치 가능]* 탭을 선택하여 아래와 같이 *Git Parameter Plug-In* 항목 선택하고 *[지금 다운로드하고 재시작 후 설치하기]* 버튼을 클릭한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-03.png)

(4) 플러그인 설치가 끝나면 본인의 *Jenkins 프로젝트 홈 화면*으로 들어가서 아래와 같이 *[구성]* 메뉴를 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-04.png)

(5) *[이 빌드는 매개변수가 있습니다]* 항목을 체크하고 *Git Parameter*를 선택한 다음 아래와 같이 내용을 작성한다.
```
Name: tag
Parameter Type: Tag
Branch: */master
```

![이미지](/files/setup-git-parameter-plugin-to-jenkins-05.png)

(6) *소스 코드 관리* 영역에서 *Branches to build* 항목의 *Branch Specifier (blank for 'any')* 항목에 아래와 같이 *${tag}*를 입력한다.
*${tag}*에서 tag는 앞에서 Git Parameter의 Name 항목에 입력한 값이다.
설정이 끝나면 아래쪽의 *[저장]* 버튼을 눌러 설정을 저장한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-06.png)

(7) 본인의 *Jenkins 프로젝트 홈 화면*으로 돌아가면 아래와 같이 Build 메뉴가 Build with Parameters 메뉴로 변경된 것을 확인할 수 있다. *Build with Parameters* 메뉴를 선택한다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-07.png)

(8) Git을 이용하여 Tag를 생성했다면 아래와 같이 생성한 Tag 목록이 나타난다.
원하는 Tag를 선택하여 *빌드하기* 버튼을 선택하면 해당 Tag 기준으로 빌드가 실행된다.

![이미지](/files/setup-git-parameter-plugin-to-jenkins-08.png)

#
{%- include package-how-to-deploy-an-application.markdown -%}
#