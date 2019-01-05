---
layout: post
title: "[애플리케이션 배포] 우분투(Ubuntu)에 MySQL 설치하기"
date: 2018-03-20 23:59:59 +0900
tags: [mysql, database, ubuntu]
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

### 1. 요구사항 확인하기
MySQL 설치 시 최소 요구 메모리를 계산하는 방법은 [여기](http://www.mysqlcalculator.com/)에서 확인한다.

### 2. MySQL 설치 명령어 실행
아래와 같이 명령어를 입력하면 MySQL을 설치할 수 있다.
```sh
$ sudo apt-get update
$ sudo apt-get install mysql-server
```

### 3. MySQL에서 사용할 비밀번호 입력
설치 중간에 root 계정의 비밀번호를 설정하는 화면이 출력된다.
비밀번호를 입력한다.

### 4. MySQL Client 실행해보기
설치가 완료되면 아래 명령어를 실행하여 MySQL DBMS를 실행해본다.
```sh
$ mysql -u root -p
```
비밀번호를 물어보면 설치할 때 설정했던 비밀번호를 입력하면 된다.<br/>
MySQL DBMS에 접속이 된다면 MySQL 설치완료!

### 5. 원격 접속 설정하는 방법
(1) 서버 컴퓨터에서 아래 명령어를 실행한다.
```sh
$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

(2) *bind-address=127.0.0.1* 항목 앞에 \#을 붙여서 주석처리 한다.

(3) 아래 명령어로 MySQL 접속한다.
```sh
$ mysql -u root -p
```

(4) 아래 SQL문 실행한다.
```sql
INSERT INTO mysql.user (host, user, authentication_string, ssl_cipher, x509_issuer, x509_subject)
VALUES ('%', 'root', password('[원하는 패스워드]'), '', '', '');
```
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
```
```sql
FLUSH PRIVILEGES;
```

(5) 아래 명령어를 실행하여 3306 포트번호로 외부에서 접속이 가능하도록 한다.
```sh
$ sudo ufw allow 3306/tcp
```
[[참조한 사이트1](https://pjt3591oo.github.io/blog/database/2017/05/03/abou_mysql_remote_connect.html)]
[[참조한 사이트2](https://zetawiki.com/wiki/MySQL_%EC%9B%90%EA%B2%A9_%EC%A0%91%EC%86%8D_%ED%97%88%EC%9A%A9)]

### 6. MySQL UTF-8 인코딩 설정방법
(1) 아래 명령어를 실행한다.
```sh
sudo vim /etc/mysql/my.cnf
```

(2) 파일 맨 아래쪽에 아래와 같이 작성한다.
```sh
[client]
default-character-set = utf8

[mysqld]
init_connect = SET collation_connection = utf8_general_ci
init_connect = SET NAMES utf8
character-set-server = utf8
collation-server = utf8_general_ci

[mysqldump]
default-character-set = utf8

[mysql]
default-character-set = utf8
```

(3) 아래 명령어를 실행하여 MySQL 서비스 재시작한다.
```sh
$ systemctl restart mysql
```

(4) 아래 명령어를 이용하여 설정이 적용됐는지 MySQL에 접속하여 확인한다.
```sh
$ mysql -u root -p
```
```sql
STATUS;
```
```sql
SHOW VARIABLES LIKE 'c%';
```

(5) 아래와 같이 입력하여 데이터베이스 인코딩 설정을 변경해준다.
```sql
ALTER DATABASE [원하는 데이터베이스명] CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

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