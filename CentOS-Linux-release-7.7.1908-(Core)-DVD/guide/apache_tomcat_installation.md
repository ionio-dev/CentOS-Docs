# Apache Tomcat
> 해당 가이드에는 Apache Tomcat 8.0.x 컴파일 설치 방법이 기재되어 있습니다.  
다른 버전을 설치해야 한다면 [Apache Archive](http://archive.apache.org/dist/tomcat/)를 참조하십시오.

<br/>
<br/>

#### ! 참고사항
> Apache Tomcat을 실행하려면 Java가 이미 설치되어있어야 합니다.  
Java 설치 가이드는 [여기](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/java_installation.md)를 참조하십시오.

<br/>
<br/>

#### Tomcat을 다운로드 합니다.
> 다운로드 경로는 서버를 구성하는 목적에 따라 달라질 수 있으며 해당 프로젝트는 /usr/local/src 하위 디렉터리에 다운로드 합니다.
```
wget http://archive.apache.org/dist/tomcat/tomcat-8/v8.0.53/src/apache-tomcat-8.0.53-src.tar.gz
```

<br/>
<br/>

#### 압축을 해제합니다.
> 해당 프로젝트에서는 /home/[사용자] 하위 디렉터리를 지정합니다.
```
tar xvzf apache-tomcat-8.0.53-src.tar.gz
```

<br/>
<br/>

#### 해당 프로젝트에서는 Tomcat 디렉토리 명을 변경하여 사용합니다.
```
mv /home/web/apache-tomcat-8.0.53-src /home/[사용자]/tomcat8
```

<br/>
<br/>

#### /etc/profile 파일에 Tomcat의 환경변수를 추가합니다.
```
vi /etc/profile
```
```
export CATALINA_HOME=/home/[사용자]/tomcat8
```

<br/>
<br/>

#### 변경사항을 적용합니다.
```
source /etc/profile
```

<br/>
<br/>

#### 환경변수가 정상적으로 등록되었는지 확인합니다.
```
echo $CALTELINA_HOME
```

<br/>
<br/>

#### firewall에 8080포트를 허용합니다.
> firewall을 사용하여 포트를 추가하는 방법은 [여기](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/preferences/firewall_settings.md)를 참조하십시오.
```
firewall-cmd --permanent --zone=public --add-port=8080/tcp
```

<br/>
<br/>

#### firewall의 변경사항을 적용합니다.
```
firewall-cmd --reload
```

<br/>
<br/>

#### 아래의 명령어로 추가된 포트를 확인할 수 있습니다.
```
firewall-cmd --list-all
```

<br/>
<br/>

#### Tomcat을 실행합니다
```
/home/web/tomcat8/bin/startup.sh
```

<br/>
<br/>

#### 만약 Permission denied가 발생한다면 해당 디렉터리로 이동하여(/home/web/tomcat8/bin) 실행권한을 부여합니다.
> 권한 부여는 파일 소유자만 가능합니다.
```
chmod 700 *.sh
```

<br/>
<br/>

#### 브라우저를 통해 접근합니다.
```
127.0.0.1:8080
```

<br/>
<br/>

# 이제 Apache Tomcat을 사용할 수 있습니다. 
<br/>
