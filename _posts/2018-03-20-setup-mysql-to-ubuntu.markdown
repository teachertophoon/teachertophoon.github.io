---
layout: post
title: "[애플리케이션 배포] 우분투(Ubuntu)에 MySQL 설치하기"
date: 2018-03-20 23:59:59 +0900
tags: [mysql, database, ubuntu]
comments: true
---
{%- include package-how-to-deploy-an-application.markdown -%}

*MySQL이 아닌 Oracle Database 설치를 원하시는 분은 [[애플리케이션 배포] Oracle Database 설치하기 (11g XE 버전 설치하기)](https://blog.tophoon.com/2018/03/14/setup-oracle-to-ubuntu.html) 글을 참고하시기 바랍니다.*

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

{%- include package-how-to-deploy-an-application.markdown -%}