# Node JS
> 해당 가이드에는 Node JS 12.13.x 버전의 컴파일 설치방법이 기재되어 있습니다.  
다른 버전을 설치해야 한다면 [Node JS Releases](https://nodejs.org/ko/download/releases/)를 참조하십시오.

<br/>
<br/>

#### ! 참고사항
> 현재 설치하는 Node JS Version 은 최소 gcc 6.x 이상이 필요합니다.
gcc 설치 가이드는 [여기](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/gcc_installation.md)를 참조하십시오.   

<br/>
<br/>

#### Node JS 를 다운로드 합니다.
> 다운로드 경로는 서버를 구성하는 목적에 따라 달라질 수 있으며 해당 프로젝트는 /home/user/download 하위 디렉터리에 다운로드 합니다.
```
wget https://nodejs.org/download/release/v12.13.1/node-v12.13.1.tar.gz
tar xfz node-v12.16.1.tar.gz
```

<br/>
<br/>

#### 압축을 풀어줍니다.
```
tar xfz node-v12.16.1.tar.gz
```

<br/>
<br/>

#### 압축이 풀린 디렉토리로 이동하여 install 설정 명령을 실행합니다.
```
cd node
./configure
```

<br/>
<br/>

#### 컴파일 설치를 실행합니다.
> make 및 make install 과정 중 현재의 사용자가 컴파일 과정 중 경유하는 경로들에 대한 권한이 없어 에러가 나는 경우가 있습니다.   
root 권한 또는 sudo 를 사용하여 설치를 진행해주십시오.
```
sudo make
sudo make install
```

<br/>
<br/>

#### 정상적을오 설치되었는지 확인합니다.
```
npm -v
node -v
```

<br/>
<br/>

# 이제 Node JS 를 사용할 수 있습니다. 
<br/>
<br/>
