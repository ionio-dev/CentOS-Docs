# MySQL 
> 해당 가이드에는 MySQL 5.7.28 버전 컴파일 설치방법이 기재되어 있습니다.

<br/>
<br/>

#### ! 필수확인
CentOS 7(DVD) 부터는 OS 설치시 기본적으로 MariaDB가 설치되어 있습니다.  
MariaDB가 설치되있는 상태로 MySQL 컴파일 설치를 진행하게 된다면 라이브러리  
충돌이 발생하여 설치가 진행되지 않을 수 있습니다. MySQL 컴파일 설치를 정상적으로  
진행하기 위해서 기본설치된 MariaDB를 삭제 후 진행하도록 합니다.  

<br/>
<br/>

#### MariaDB 설치확인
```
rpm -qa | maraidb
```

<br/>
<br/>

#### MariaDB 제거
```
yum remove mariadb*
```

<br/>
<br/>

#### ! 참고사항
MySQL을 컴파일 설치하기 이전에 openssl 버전 및 지원내용을 확인하는 것을 권장합니다.  
현재 진행 중인 프로젝트에서는 openssl-1.0.2k가 기본 설치되어 있는데 공식 [홈페이지](https://www.openssl.org/)에서는  
현재 날짜(2019.11)를 기준으로 LTS 버전(1.0.2 시리즈)은 2019 년 12 월 31 일까지만  
지원되기 때문에 최신 안정 버전인 1.1.1 버전으로 업그레이드하는 것을 권장한다고 합니다.  

- OpenSSL 버전 업그레이드 방법은 [여기](https://github.com/ionio-dev/Dev-Docs/tree/master/OperatingSystem/Linux/Redhet/CentOS/CentOS-Linux-release-7.7.1908-(Core)-DVD/Reference/OpenSSL/OpenSSL_Guide.md)를 참조하십시오.

<br/>
<br/>

#### MySQL 설치를 위해 필수 패키지를 설치합니다.
```
yum -y install gcc gcc-c++ ncurses ncurses-devel
```

<br/>
<br/>

#### 설치하고자 하는 MySQL 버전이 5.5 이상이라면 은 CMake를 사용하여 makefile을 생성합니다.
```
yum install cmake
```

<br/>
<br/>

#### 패키지를 다운로드할 경로로 이동합니다.
> 해당 프로젝트에서는 /usr/local 하위 디렉터리에 다운로드합니다.
```
cd /usr/local
```

<br/>
<br/>

#### 설치하고자 하는 MySQL 버전이 5.7 이상이라면 [Boost](https://ko.wikipedia.org/wiki/Boost) 라이브러리를 필요로 합니다.
> MySQL 5.7 이상부터는 기하 연산(GIS)를 위해서 Boost 라이브러리의 Geometry를 사용합니다.
```
wget http://downloads.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz
```
```
tar xvfz boost_1_59_0.tar.gz
```

<br/>
<br/>

#### [MySQL Community Server](https://dev.mysql.com/downloads/mysql/)에 접속하여 원하는 버전의 MySQL 링크를 복사하여 다운로드합니다.
> 해당 가이드에서는 MySQL 5.7 Source Code를 다운로드해 컴파일 설치를 진행하는 방법을 기재합니다.
```
https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.28.tar.gz
```
```
tar xvfz mysql-5.7.28.tar.gz
```

<br/>
<br/>

#### 압축 해제된 디렉터리로 이동합니다.
```
cd mysql-5.7.28
```

<br/>
<br/>

#### 컴파일 옵션을 지정해 makefile을 생성해줍니다.
> **경고!** 설치 옵션에 있는 경로들은 로컬 환경에 맞게 설정해주십시오.
```
cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DWITH_EXTRA_CHARSETS=all \
-DMYSQL_DATADIR=/home/mysql_data \
-DENABLED_LOCAL_INFILE=1 \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=../boost_1_59_0 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DENABLED_LOCAL_INFILE=1 \
-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
-DSYSCONFDIR=/etc \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS=all \
-DWITH_SSL=/usr/bin/openssl
```

<br/>
<br/>

#### 생성된 makefile을 컴파일 합니다.
```
make
```

<br/>
<br/>

#### 컴파일이 완료되었다면 설치를 진행합니다.
```
make install
```

<br/>
<br/>

#### 설치가 완료되었다면 MySQL 사용자를 추가 및 권한 설정을 합니다.
```
useradd -M mysql -u 27 >& /dev/null
```
```
chown -R root:mysql /usr/local/mysql
```
```
cd /usr/local/mysql
```
```
chmod 700 support-files/mysql.server
```

<br/>
<br/>

#### 환경설정 파일을 복사 및 심볼릭 링크를 설정합니다.
```
cp support-files/mysql.server /etc/rc.d/init.d/mysql
```
```
cp support-files/mysql.server /usr/bin/
```
```
ln -s /etc/rc.d/init.d/mysql /etc/rc.d/rc3.d/S97mysql
```

<br/>
<br/>

#### my.cnf 파일을 작성합니다.
> MySQL 5.7 이상부터는 my.cnf 파일을 직접 작성합니다.
```
vi /etc/my.cnf
```

<br/>
<br/>

#### 해당 가이드는 Innodb 기준으로 my.cnf 파일을 작성합니다.
```
[client]
default-character-set = utf8
port = 3306
socket = /tmp/mysql.sock
default-character-set = utf8

[mysqld]
socket=/tmp/mysql.sock
datadir=/home/mysql_data
basedir = /usr/local/mysql
#user = mysql
#bind-address = 0.0.0.0
skip-external-locking
key_buffer_size = 384M
max_allowed_packet = 1M
table_open_cache = 512
sort_buffer_size = 2M
read_buffer_size = 2M
read_rnd_buffer_size = 8M
myisam_sort_buffer_size = 64M
thread_cache_size = 8
query_cache_size = 32M
#dns query
skip-name-resolve
#connection
max_connections = 1000
max_connect_errors = 1000
wait_timeout= 60
#slow-queries
#slow_query_log = /home/mysql_data/slow-queries.log
#long_query_time = 3
#log-slow-queries = /home/mysql_data/mysql-slow-queries.log
##timestamp
explicit_defaults_for_timestamp
symbolic-links=0
### log
log-error=/home/mysql_data/mysqld.log
pid-file=/tmp/mysqld.pid
###chracter
character-set-client-handshake=FALSE
init_connect = SET collation_connection = utf8_general_ci
init_connect = SET NAMES utf8
character-set-server = utf8
collation-server = utf8_general_ci
symbolic-links=0
##Password Policy
#validate_password_policy=LOW
#validate_password_policy=MEDIUM
### MyISAM Spectific options
##default-storage-engine = myisam
key_buffer_size = 32M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
### INNODB Spectific options
default-storage-engine = InnoDB
#skip-innodb
#innodb_additional_mem_pool_size = 16M
innodb_buffer_pool_size = 1024MB
innodb_data_file_path = ibdata1:10M:autoextend
innodb_write_io_threads = 8
innodb_read_io_threads = 8
innodb_thread_concurrency = 16
innodb_flush_log_at_trx_commit = 1
innodb_log_buffer_size = 8M
innodb_log_file_size = 128M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120

[mysqldump]
default-character-set = utf8
max_allowed_packet = 16M

[mysql]
no-auto-rehash
default-character-set = utf8

[myisamchk]
key_buffer_size = 256M
sort_buffer_size = 256M
read_buffer = 2M
write_buffer = 2M
```

<br/>
<br/>

#### 설정 파일을 생성하였다면 mysql_install_db를 실행합니다.
```
/usr/local/mysql/bin/mysql_install_db --user=mysql --datadir=/home/mysql_data
```

<br/>
<br/>

아래의 경고는 무시하여도 괜찮습니다.
> [WARNING] mysql_install_db is deprecated. Please consider switching to mysqld --initialize

<br/>
<br/>

#### /home/mysql_data 디렉터리에 파일이 생성되었는지 확인합니다.
```
ls -l /home/mysql_data
```

<br/>
<br/>

#### mysql 명령어를 시스템 전체에서 사용할 수 있도록 실행 파일을 복사합니다.
```
cp -apr mysql /usr/bin/
```

<br/>
<br/>

#### 설치가 성공적으로 완료되었다면 MySQL 을 실행합니다.
```
mysql.server start
```

<br/>
<br/>

#### MySQL root 접속을 위해서 패스워드를 확인합니다.
> 최초의 root 암호 파일은 해당 작업자(사용자) 디렉터리에 .mysql_secret 파일에 저장되어 있습니다.
```
cat /root/.mysql_secret
```

<br/>
<br/>

#### 확인한 패스워드로 mysql에 진입합니다.
```
mysql -u root -p
Enter password: [패스워드입력]
```

<br/>
<br/>

#### root의 패스워드를 변경한 뒤 적용합니다.
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '변경할 패스워드';
mysql> flush privileges;
```

<br/>
<br/>

#### 로그아웃한 뒤에 mysql 변경한 패스워드로 재접속 하여 characterset을 확인해봅니다.
```
mysql> \s
--------------
mysql  Ver 14.14 Distrib 5.7.28, for Linux (x86_64) using  EditLine wrapper

Connection id:          17
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.7.28 Source distribution
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    utf8
Db     characterset:    utf8
Client characterset:    utf8
Conn.  characterset:    utf8
UNIX socket:            /tmp/mysql.sock
Uptime:                 21 min 52 sec

Threads: 1  Questions: 11  Slow queries: 0  Opens: 115  Flush tables: 1  Open tables: 40  Queries per second avg: 0.008
--------------
```

<br/>
<br/>

# 이제 MySQL을 사용할 수 있습니다.

<br/>
<br/>

#### MySQL 관련 가이드를 작성하기 위해서 해당 사이트를 참고 인용하였습니다.
- [mysql 5.7 install (comfile)](https://xinet.kr/?p=991)
