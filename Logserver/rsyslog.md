
## Server

# rsyslog 설치

'''bash
$ wget http://rpms.adiscon.com/v8-stable/rsyslog.repo
$ mv rsyslog.repo /etc/yum.repos.d/rsyslog.repo
$ yum install -y rsyslog
$ systemctl enable rsyslog
'''
rsyslog -mysql설치
'''bash
$ yum install -y rsyslog-mysql
'''

# rsyslogDB세팅

mysql설치
'''bash
$ wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
$ sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
$ yum update
$ yum install -y mysql-server
$ systemctl start mysqld
'''
mysql초기 설정

'''bash
$ mysql_secure_installation
'''

*rsyslog database 덤프
-rsyslog-mysql의 createDB.sql경로 확인

Rsyslog용 계정 생성 





