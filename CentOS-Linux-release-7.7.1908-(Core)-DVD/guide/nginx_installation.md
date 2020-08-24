# Nginx
> 해당 가이드에는 Nginx 1.19.x 컴파일 설치 방법이 기재되어 있습니다.  
다른 버전을 설치해야 한다면 [Nginx Download](https://nginx.org/en/download.html)를 참조하십시오.

<br/>
<br/>

#### ! 참고사항
> Nginx 를 설치하려면 pcre 와 zlib 가 이미 설치되어있어야 합니다.  
설치 가이드는 [pcre](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/pcre_installation.md) 와 
[zlib](https://github.com/ionio-dev/CentOS-Docs/blob/master/CentOS-Linux-release-7.7.1908-(Core)-DVD/guide/zlib_installation.md) 를 참조하십시오.

<br/>
<br/>

#### Nginx 를 다운로드 및 압출을 해제합니다.
> 다운로드 경로는 서버를 구성하는 목적에 따라 달라질 수 있으며 해당 프로젝트는
/home/user/nginx 하위 디렉터리에 다운로드 합니다.
```
wget https://nginx.org/download/nginx-1.19.2.tar.gz
tar xvfz nginx-1.19.2.tar.gz
```

<br/>
<br/>

#### 압축이 풀린 디렉토리로 이동하여 install 설정 명령을 실행합니다.
```
cd nginx-1.6.2

./configure \
--prefix=/home/user/nginx \
--user=daemon \
--group=daemon \
--with-openssl=/usr/bin
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

#### Nginx 설정파일을 수정합니다.
> 사용하는 용도에 따라 설정방법이 다양합니다.   
[여기](https://12bme.tistory.com/366)를 참조하십시오.
```
cd /home/user/nginx/conf
vi nginx.conf
```

<br/>
<br/>

#### Nginx 실행 및 중지 테스트를 진행합니다.
```
cd /home/user/nginx/sbin
./nginx
./nginx -s stop
```

<br/>
<br/>

# 이제 Node JS 를 사용할 수 있습니다. 
<br/>
<br/>


