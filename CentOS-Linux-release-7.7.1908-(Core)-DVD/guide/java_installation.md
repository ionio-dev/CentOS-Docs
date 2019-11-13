# JAVA (OpenJDK)
> 해당 가이드에는 Oracle사의 JAVA가 아닌 [OpenJDK](https://ko.wikipedia.org/wiki/OpenJDK)를 설치하는 방법이 기재되어 있습니다.  

<br/>
<br/>

#### 설치 가능한 OpenJDK 목록을 조회합니다.
```
yum list java-*-open*
```

<br/>
<br/>

#### 출력된 목록에서 원하는 버전의 OpenJDK를 설치합니다.
> 해당 프로젝트에서는 OpenJDK 1.8 버전을 설치합니다.
```
yum install java-1.8.0-openjdk-devel.x86_64
```

<br/>
<br/>

#### ! 참고사항
> Linux 환경에서도 JDK와 JRE는 별도의 패키지로 구분됩니다. *-devel 패키지가 JDK 이며  
JDK는 JRE에 의존성이 걸려있어 JDK설치시 JRE가 자동으로 먼저 설치됩니다.

<br/>
<br/>

#### yum 으로 설치하게되면 버전관리 관련 디렉토리(/etc/alternatives)에 자동으로 링크 등록이 됩니다.
> 만약 수동으로 **컴파일 설치하게 된다면 alternatives명령어로 직접 링크 생성 및 설정**합니다.
```
alternatives --list | grep java
```

<br/>
<br/>

#### Java가 버전 별로 설치되어 있다면 alternatives 명령어로 원하는 사용하는 버전을 선택할 수 있습니다.
> '+' 가 붙은 행이 현재 선택된 버전입니다.
```
alternatives --config java
```

<br/>
<br/>

#### OpenJDK 기본 경로를 /etc/profile 파일에 Java환경변수를 등록해줍니다.
> yum 으로 설치시 기본적으로 /usr/lib/jvm 하위에 링크되어 있습니다.
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64
```

<br/>
<br/>

#### 변경사항을 적용합니다.
```
source /etc/profile
```

<br/>
<br/>

#### 정상적으로 설치가 완료되었는지 확인합니다.
```
java -version
javac -version
```

<br/>
<br/>

#### 환경변수가 정상적으로 등록되었는지 확인합니다.
```
echo $JAVA_HOEM
```

<br/>
<br/>

# 이제 Java를 사용할 수 있습니다.
<br/>
