---
layout: post
title: "[애플리케이션 배포] Oracle Database 설치하기 (11g XE 버전 설치하기)"
date: 2018-03-14 23:59:59 +0900
tags: [oracle, database, ubuntu]
comments: true
---
## 1. Oracle Database 설치파일 내려받기
Oracle Database도 역시 우분투(Ubuntu) 운영체제를 위한 공식적인 패키지를 제공하지 않는다.<br/>
아래 명령을 실행하여 3rd party 제조사의 소프트웨어 설치 시 보통 사용하는 폴더로 이동한다.
```sh
$ cd /opt
```
크롬 웹브라우저를 실행하여 [여기](http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html)로 이동하여 
본인의 컴퓨터에 맞는(예: Linux x64) Oracle Database 설치파일을 다운로드 받는다.

다운로드 도중에 *Chrome 오른쪽 상단 ...(세로로) 버튼*을 누른 후 *다운로드* 메뉴를 선택한다.
다운로드 중인 항목을 마우스 오른쪽 버튼으로 누른 후 *링크 주소 복사* 메뉴 클릭한다.

아래와 같이 wget 명령어와 복사한 링크 주소를 이용하여 Oracle Database 설치파일을 /opt 폴더에 내려받는다.
```sh
$ sudo wget [크롬 웹브라우저에서 복사했던 링크 주소]
```
이와같이 Oracle Database 설치파일을 내려받는 이유는 아래와 같다.
- Oracle에서 Ubuntu용 설치파일을 공식적으로 제공하지 않기 때문이다.
- 크롬 웹브라우저에서 복사했던 링크 주소가 보안을 위해 일정기간 동안만 사용할 수 있는 주소이기 때문이다.

## 2. RPM(Redhat Package Manager)파일을 DEB(Debian)패키지 파일로 변경하기
리눅스(Linux) 운영체제는 크게 레드햇(Redhat) 계열과 데비안(Debian) 계열로 나뉜다.
내려받은 Oracle Database 설치파일은 레드햇 계열인 rpm 파일이고, 우분투는 데비안 계열의 운영체제이기 때문에 내려받은 설치파일 그대로 이용할 수 없다.
레드햇 계열의 대표적인 운영체제로는 RHEL, CentOS가 있다는 것을 참고로 알아두면 좋다.

따라서 우리는 rpm 파일을 deb 파일로 변경해줘야 한다.

우선 아래 명령어를 이용하여 내려받은 파일명을 변경한다.
```
$ sudo mv -- [내려받은 파일명] oracle-xe-11.2.0-1.0.x86_64.rpm.zip
```

zip 압축을 풀기위한 unzip 프로그램 설치를 위해 아래와 같은 명령을 실행한다.
```sh
$ sudo apt install unzip
```

unzip 프로그램 설치가 끝나면 아래 명령을 이용하여 압축을 해제한다.
```sh
$ sudo unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
```

압축을 해제하면 Disk1 디렉토리가 생성된다. 아래 명령을 이용하여 Disk1 디렉토리로 이동한다.
```sh
$ cd Disk1
```

rpm 파일을 deb 파일로 변환하기 위한 프로그램을 아래 명령어를 이용하여 설치한다.
```sh
$ sudo apt-get install alien libaio1 unixodbc
```

rpm 파일을 deb 파일로 변환하기 위한 명령어를 아래와 같이 실행한다.
실행하는데 시간이 오래 걸리기 때문에 인내심을 가지고 기다려야 한다.
```sh
$ sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
```

## 3. Oracle Database 설치 전 환경변수 설정하기
아래 명령어를 이용하여 bc를 설치한다.
```sh
$ sudo apt-get install bc
```

아래 명령어를 실행하여 chkconfig 파일을 생성한다.
```sh
$ sudo vim /sbin/chkconfig
```
편집기가 열린 상태에서 아래 내용을 작성한다.
다 작성했다면 *:wq*를 눌러 저장하고 편집화면에서 나온다.
```sh
#!/bin/bash
# Oracle 11gR2 XE installer chkconfig hack for Ubuntu
file=/etc/init.d/oracle-xe
if [[ ! `tail -n1 $file | grep INIT` ]]; then
echo >> $file
echo '### BEGIN INIT INFO' >> $file
echo '# Provides: OracleXE' >> $file
echo '# Required-Start: $remote_fs $syslog' >> $file
echo '# Required-Stop: $remote_fs $syslog' >> $file
echo '# Default-Start: 2 3 4 5' >> $file
echo '# Default-Stop: 0 1 6' >> $file
echo '# Short-Description: Oracle 11g Express Edition' >> $file
echo '### END INIT INFO' >> $file
fi
update-rc.d oracle-xe defaults 80 01
```

생성한 chkconfig 파일에 권한을 아래 명령을 이용하여 설정한다.
```sh
$ sudo chmod 755 /sbin/chkconfig
```

아래 명령어를 이용하여 60-oracle.conf 파일을 생성한다.
```sh
$ sudo vim /etc/sysctl.d/60-oracle.conf
```

생성한 다음 아래 내용을 작성하고 *:wq*를 이용하여 저장한다.
아래 설정 값들은 [여기](https://docs.oracle.com/cd/E17781_01/install.112/e18802/toc.htm)를 참고한 것이다.
```sh
# Oracle 11g XE kernel parameters
fs.file-max=6815744  
net.ipv4.ip_local_port_range=9000 65000  
kernel.sem=250 32000 100 128 
kernel.shmmax=536870912
```

아래 명령어를 입력하여 60-oracle.conf 파일에 입력했던 내용이 정확하게 입력했는지 확인한다.
```sh
$ sudo cat /etc/sysctl.d/60-oracle.conf
```

생성한 파일을 적용하기 위해 procps 서비스를 아래 명령어를 이용하여 재시작한다.
```sh
$ sudo service procps restart
```

아래 명령어를 실행하여 설정파일 내용과 일치하는지 확인해본다. 일치한다면 서비스가 정상적으로 재시작되었다는 뜻이다.
```sh
$ sudo sysctl -q fs.file-max
```

아래 명령어를 이용하여 swap space(가상메모리) 값을 확인한다.
swap space 용량을 확인 후 Oracle Database 설치 가이드에 나와있는대로 2GB 이상이면 그대로 둔다.
```sh
free -m
```

Oracle XE 버전은 /bin/awk를 사용하는데 Ubuntu에는 /usr/bin/awk에 설치되어 있기 때문에 아래와 같이 명령을 실행하여 심볼릭링크(바로가기)를 만들어준다.
```sh
$ sudo ln -s /usr/bin/awk /bin/awk
```

아래 명령을 실행하여 Oracle XE 리스너가 사용할 lock 파일을 만들어준다.
```sh
$ sudo mkdir /var/lock/subsys
$ sudo touch /var/lock/subsys/listener
```

## 4. Oracle Database 설치하기
아래 명령어를 실행하여 Oracle XE 설치를 진행한다.
```sh
$ sudo dpkg --install oracle-xe_11.2.0-2_amd64.deb
```

## 5. 설치 후 환경변수 설정하기
설치가 완료되면 아래 명령어를 이용하여 Oracle XE의 Port 설정과 SYSTEM 비밀번호 지정을 한다.
```sh
$ sudo /etc/init.d/oracle-xe configure
```
- 아래 화면에서 첫번째 노란색 부분은 Oracle Database에서 사용할 포트번호를 설정하는 것이다. 8080 포트를 그대로 사용할 것이라면 엔터키를 입력하면 된다.
- 두번째 노란색 부분은 Database Listener 포트번호를 설정하는 것이다. 1521 포트를 그대로 사용할 것이라면 엔터키를 입력한다.
- 세번째 노란색 부분에서는 원하는 SYSTEM 비밀번호(root 계정 비밀번호)를 입력하고 엔터키를 입력한다.
- 네번째 노란색 부분은 서버가 부팅될 때 Oracle Database 자동 실행여부를 물어보는 것인데, 자동 실행할 것이기 때문에 y를 입력하고 엔터키를 입력한다.

![이미지](/files/setup-oracle-to-ubuntu-01.png)

아래 명령어를 실행한 후 아래 내용을 맨 마지막에 추가한다.
```sh
$ sudo vim ~/.bashrc
```
```sh
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
export ORACLE_SID=XE
export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh`
export ORACLE_BASE=/u01/app/oracle
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME/bin:$PATH
```

아래 명령어를 실행하여 변경사항을 우분투에게 알려준다.
```sh
$ source ~/.bashrc
```

아래 명령어를 실행하여 Oracle XE 서비스를 재시작 시켜준다.
```sh
$ sudo service oracle-xe start
```

아니면, 아래 명령어를 입력하여 서버를 재부팅하는 것도 한가지 방법이다.
```sh
$ sudo reboot
```

sqlplus 명령어를 실행한 다음, 사용자 등록 후 평소 본인의 컴퓨터에서 사용하듯이 사용하면 된다.
Oracle Database 사용자 등록하는 방법은 아래와 같다.
```sh
# Oracle Database Client 실행(sqlplus [아이디]/[비밀번호])
$ sqlplus system/[설치할 때 설정한 비밀번호 입력]
```
```sql
-- 관리자 권한 접속(sys/oracle)
CONN SYS AS SYSDBA;

-- 사용자 생성(ID: tophoon / PW: 1234)
CREATE USER tophoon IDENTIFIED BY 1234;

-- 권한 주기(연결, 리소스 접근, DBA 권한을 tophoon 사용자에게 부여하겠다는 뜻)
GRANT CONNECT, RESOURCE, DBA TO tophoon;

-- 현재 Oracle Database Client에 접속한 사용자 확인하는 명령어
SHOW USER;
```

## 6. 원격 접속 설정하는 방법
Oracle DBMS 외부접속을 허용하기 위해 1521 포트번호 접속을 허용하도록 아래 명령어를 실행한다.
```sql
$ sudo ufw allow 1521/tcp
```
Eclipse IDE를 실행하여 Data Source Explorer 창에서 서버의 데이터베이스를 등록하여 동작하는지 확인해본다.
여기까지 하면 Oracle Database 설치가 완료된 것이다.
