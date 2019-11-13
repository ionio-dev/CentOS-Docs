# Apache
> 해당 가이드에는 Apache 2.4.x 버전 컴파일 설치방법이 기재되어 있습니다.

<br/>
<br/>

#### ! 참고사항
Apache를 컴파일 설치하기 이전에 openssl 버전 및 지원내용을 확인하는 것을 권장합니다.  
현재 진행 중인 프로젝트에서는 openssl-1.0.2k가 기본 설치되어 있는데 공식 홈페이지에서는  
현재 날짜(2019.11)를 기준으로 LTS 버전(1.0.2 시리즈)은 2019 년 12 월 31 일까지만  
지원되기 때문에 최신 안정 버전인 1.1.1 버전으로 업그레이드하는 것을 권장한다고 합니다.  
- OpenSSL 버전 업그레이드 방법은 [여기](https://github.com/ionio-dev/Dev-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/preferences/openssl_settings.md)를 참조하십시오.

<br/>
<br/>

#### Apache가 이미 설치되어 있을 수도 있습니다.
> 설치되어 있다면 라이브러리 충돌을 방지하기 위해 제거 후 설치를 진행하는 것을 권장합니다.
```
yum remove -y httpd httpd-*
```

<br/>
<br/>

#### Apache를 설치하는데 필요한 패키지를 다운로드합니다.
```
yum install -y make gcc g++ gcc-c++ autoconf automake libtool pkgconfig findutils oepnssl openssl-devel openldap-devel pcre-devel libxml2-devel lua-devel curl curl-devel libcurl-devel expat-devel flex
```

<br/>
<br/>

#### 원하는 버전의 아파치 패키지와 의존 라이브러리를 [미러서버](http://mirror.apache-kr.org/)에서 다운로드 합니다.
> Apache 2.4.x 버전부터는 apr과 apr-util,pcre 를 별도로 설치하여야 apache 설치가 완료됩니다.  
해당 프로젝트에서 Apache 소스 파일의 다운로드 경로는 /usr/local/src 하위 디렉터리를 사용합니다.
```
wget http://mirror.apache-kr.org/httpd/httpd-2.4.41.tar.gz
wget http://mirror.apache-kr.org/apr/apr-1.6.5.tar.gz
wget http://mirror.apache-kr.org/apr/apr-util-1.6.1.tar.gz
wget http://downloads.sourceforge.net/project/pcre/pcre/8.41/pcre-8.41.tar.gz
```
```
tar xvf apr-1.6.5.tar.gz
tar xvf apr-util-1.6.1.tar.gz
tar xvf httpd-2.4.41.tar.gz
tar xvf pcre-8.41.tar.gz
```

<br/>
<br/>

#### apr와 apr-util폴더를 httpd 하위의 지정 경로로 이동시킵니다.
```
mv apr-1.6.5 ./httpd-2.4.41/srclib/apr
mv apr-util-1.6.1 ./httpd-2.4.41/srclib/apr-util
```

<br/>
<br/>

#### pcre-8.41 디렉터리 안으로 이동하여 설정 및 makefile을 생성합니다.
```
./configure
```

<br/>
<br/>

#### pcre을 컴파일 및 설치합니다.
```
make && make install
```

<br/>
<br/>

#### httpd 디렉터리 안으로 이동하여 설정 및 makefile생성후 설치합니다.
> 옵션 설정의 경로는 구성에 따라 원하는 경로로 설정합니다.
```
./configure --prefix=/usr/local/apache2 --enable-so --enable-ssl --with-ssl=/usr/local/ssl --with-included-apr --with-included-apr-util
```

<br/>
<br/>

#### httpd를 컴파일 및 설치합니다.
```
make && make install
```

<br/>
<br/>

#### prefix 옵션에 지정했던 경로 하위에 생성된 httpd.conf 파일에서 ServerName 항목을 수정합니다.
```
vi /usr/local/apache2/conf/httpd.conf
```
```
ServerName 127.0.0.1:80
```

<br/>
<br/>

#### systemctl에 apache.service 파일을 생성합니다.
```
vi /etc/systemd/system/apache.service
```

<br/>
<br/>

#### apache.service 내용을 추가합니다.
```
[Unit]
Description=The Apache HTTP Server

[Service]
Type=forking
PIDFile=/usr/local/apache2/logs/httpd.pid
ExecStart=/usr/local/apache2/bin/apachectl start
ExecReload=/usr/local/apache2/bin/apachectl graceful
ExecStop=/usr/local/apache2/bin/apachectl stop
KillSignal=SIGCONT
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

<br/>
<br/>

#### 데몬을 을 재시작 합니다.
```
systemctl daemon-reload
```

<br/>
<br/>

#### Apache를 시스템 부팅시 자동 활성화 되도록 설정 합니다.
```
systemctl enable apache
```

<br/>
<br/>

#### 방화벽 설정에서 80번 포트를 등록한 뒤에 재시작 합니다.
> 방화벽 설정에 자세한 내용은 [여기](https://github.com/ionio-dev/Dev-Docs/blob/master/OperatingSystem/Linux/Redhet/CentOS/CentOS-Linux-release-7.7.1908-(Core)-DVD/Reference/Firewall/Firewall_Guide.md)를 참조하십시오.
```
firewall-cmd --permanent --zone=public --add-port=80/tcp
```
```
firewall-cmd --reload
```

<br/>
<br/>

#### Apache Service를 실행합니다.
```
systemctl start apache.service
```

<br/>
<br/>

#### 시스템 부팅시 Apache Service가 자동 활성화 되도록 설정 합니다.
```
systemctl enable apache.service
```

<br/>
<br/>

#### Apache Service가 자동 활성화 되도록 설정되어있는지 확인할 수 있습니다.
```
systemctl list-unit-files --type=service | grep apache.service
```

<br/>
<br/>

#### Apache를 설치한 서버의 ip정보를 확인합니다.
> ifconfig 명령어는 net-tools 패키지가 설치되어있어야 작동합니다.  
```
ifconfig
```

<br/>
<br/>

#### 브라우저에서 확인된 아이피:포트 주소로 접속 시 It works!라는 문구를 확인할 수 있습니다.
```
xxx.xxx.xxx.xxx:80
```

<br/>
<br/>

#### 프로세스에서 httpd의 상태를 볼 수 있습니다.
```
ps -ef | grep httpd
```

<br/>
<br/>

#### Apache.service파일을 생성하였다면 Apache 상태를 확일할 수 있습니다.
```
systemctl status apache
```

<br/>
<br/>

# 이제 Apache를 사용할 수 있습니다.
<br/>
