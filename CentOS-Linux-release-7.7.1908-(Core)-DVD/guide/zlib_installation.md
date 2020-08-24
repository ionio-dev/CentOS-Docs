# ZLIB
> zlib 는 gzip 등으로 압축된 파일을 읽고 쓰기 위해 필요한 Library 입니다. 
해당 가이드에서는 1.2.8 version 컴파일 설치 방법이 기재되어 있습니다.

<br/>
<br/>

#### zlib 을 다운로드 및 압출을 해제합니다.
> 다운로드 경로는 서버를 구성하는 목적에 따라 달라질 수 있으며 해당 프로젝트는   
/home/user/download 하위 디렉터리에 다운로드 합니다.
```
wget https://zlib.net/fossils/zlib-1.2.8.tar.gz
tar xvfz zlib-1.2.8.tar.gz
```

<br/>
<br/>

#### 압축이 풀린 디렉토리로 이동하여 install 설정 명령을 실행합니다.
> 해당 프로젝트에서는 nginx 컴파일 설치를 위해 prefix 경로를 임의 지정 경로로 입력합니다.
```
cd zlib-1.2.8
./configure --prefix=/home/coninfo/nginx
```

<br/>
<br/>

#### 컴파일 설치를 실행합니다.
> make 및 make install 과정 중 현재의 사용자가 컴파일 과정 중 경유하는 경로들에 대한 권한이 없어 에러가 나는 경우가 있습니다.   
root 권한 또는 sudo 를 사용하여 설치를 진행해주십시오.
```
make
make install
```

<br/>
<br/>
