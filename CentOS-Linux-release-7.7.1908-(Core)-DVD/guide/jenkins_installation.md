# Jenkins
> 해당 가이드는 작성일자 기준 LTS Version (2.235.5) 으로 컴파일 설치 방법이 기재되어 있습니다.   
자세한 사항은 [Jenkins Download](https://www.jenkins.io/download/)를 참조 하십시오.

<br/>
<br/>

#### ! 참고사항
> jenkins 를 실행하려면 tomcat이 미리 설치되어있어야 합니다.  
tomcat 설치 가이드는 [여기](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/apache_tomcat_installation.md)를 참조하십시오.

<br/>
<br/>

#### Jenkins 를 다운로드 합니다.
> 다운로드 경로는 서버를 구성하는 목적에 따라 달라질 수 있으며 해당 프로젝트는 /home/user/download 하위 디렉터리에 다운로드 합니다.
```
wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
```

<br/>
<br/>

#### Jenkins.war 파일을 이동합니다.
> 설치된 tomcat 의 webapps 디렉토리 안으로 jenkins.war 파일을 이동합니다.
```
mv jenkins.war /home/user/tomcat/wepapps
```

<br/>
<br/>

#### Tomcat 의 server.xml 파일을 수정합니다.
> 설치된 tomcat 의 conf 디렉토리 안에 server.xml 을 열어 Host 하위에 Context 를 프로젝트 기준에 맞게 추가합니다.   
Context 의 path 는 원하는 값으로 지정하시면 됩니다.
```
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="true">
  <Context path="/jenkins" debug="0" privileged="true" docBase="jenkins.war" />
</Host>
```

<br/>
<br/>

#### Jenkins 를 실행합니다.
> 설정한 tomcat 을 실행하게 되면 tomcat 을 실행한 User 의 home 디렉토리에 .jenkins 디렉토리가 생성되며   
그곳이 바로 <b>jenkins_home</b> 디렉토리 입니다.   

<br/>
<br/>

#### initialAdminPassword
> 최초 jenkins 가 실행될 때에 initialAdminPassword 를 입력하여야 합니다.   
해당 password 는 .jenkins/secrets/initialAdminPassword 파일에 기재되어 있습니다.
```
cat /home/user/.jenkins/secrets/initialAdminPassword
```

<br/>
<br/>

# 이제 Jenkins 를 사용할 수 있습니다. 
<br/>
