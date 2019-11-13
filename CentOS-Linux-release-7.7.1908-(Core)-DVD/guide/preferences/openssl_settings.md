# OpenSSL (TLS-SSL)
> 해당 가이드에서는 TLS(SSL)의 상세한 정보를 제공하기보다는 현재 날짜(2019.11)를 기준으로 설치(적용) 되어있는  
TLS(SSL) 버전의 **업그레이드 가이드**가 기재되어 있습니다.

<br/>
<br/>

*[TLS란](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EA%B3%84%EC%B8%B5_%EB%B3%B4%EC%95%88) Layer Security의 약자로  컴퓨터 네트워크에 통신 보안을 제공하기 위해 설계된 암호 규약이다.  
그리고 Transport Layer Security(TLS)이라는 이름은 Secure Sockets Layer가 표준화되면서 바뀐 이름이다.  
이 규약은 인터넷 같이 TCP/IP 네트워크를 사용하는 통신에 적용되며, 통신 과정에서 전송계층 종단 간  
보안과 데이터 무결성을 확보해준다.*  

<br/>
<br/>

**공식 홈페이지 지원내용에 현재 LTS 버전(1.0.2 시리즈)은 2019 년 12 월 31 일까지만 지원되기 때문에  
최신 안정 버전인 1.1.1 버전으로 업그레이드하는 것을 권장한다고 합니다.**
> OpenSSL 공식 [홈페이지](https://www.openssl.org/)에 릴리즈 노트 및 관련 정책이 잘 기재되어 있으니 사용(또는 업데이트)  
이전에 참고하여 진행하는것을 권장하며 CentOS 7 기준으로 기본 OpenSSL 1.0.2k가 설치되어 있습니다.

<br/>
<br/>

```
Note: The latest stable version is the 1.1.1 series.  
This is also our Long Term Support (LTS) version, supported until 11th September 2023.  
Our previous LTS version (1.0.2 series) will continue to be supported until 31st December 2019  
(security fixes only during the last year of support).  

The 1.1.0 series is currently only receiving security fixes and will go out of support on 11th September 2019.  
All users of 1.0.2 and 1.1.0 are encouraged to upgrade to 1.1.1 as soon as possible.  
The 0.9.8, 1.0.0 and 1.0.1 versions are now out of support and should not be used.
```

<br/>
<br/>
<br/>

#### 패키지 소스를 설치할 경로로 이동하여 원하는 1.1.1 이상의 버전을 다운로드 합니다.
> 해당 가이드에서는 기본 설치 패키지 파일 및 유틸리티 설치경로는 /usr/local 사용합니다.
```
wget https://www.openssl.org/source/openssl-1.1.1d.tar.gz
```

<br/>
<br/>

#### 파일을 압축 해제합니다.
```
tar xzvf openssl-1.1.1d.tar.gz
```

<br/>
<br/>

#### 생성된 openssl-1.1.1d 디렉터리로 이동하여 makefile을 생성합니다.
> config 옵션은 [여기](https://wiki.openssl.org/index.php/Compilation_and_Installation#Configure_Options)를 참조하십시오.
```
./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl shared
```

<br/>
<br/>

#### 컴파일을 실행해줍니다.
```
make
```

<br/>
<br/>

#### 컴파일이 완료되었다면 컴파일된 소스를 설치합니다.
> 설치가 완료되면 /usr/local 디렉터리에 ssl 디렉터리에 생성된 것을 확인할 수 있습니다.  
```
make install
```

<br/>
<br/>

#### 동적 라이브러리 설정 파일을 /etc/ld.so.conf.d 하위 경로에 생성해줍니다.
```
vi /etc/ld.so.conf.d/openssl-1.1.1d.conf
```

<br/>
<br/>

#### openssl-1.1.1d.conf 에 설정할 내용은 아래와 같습니다.
```
/usr/local/ssl/lib
```

<br/>
<br/>


#### 생성한 .conf 파일을 적용시켜줍니다.
```
ldconfig -v
```

<br/>
<br/>


#### 동적 라이브러리에 링크를 걸어줍니다.
> 필수는 아니지만 접근성을 위해 진행합니다.
```
ln -s /usr/local/ssl/lib/libssl.so.1.1 /usr/lib64/libssl.so.1.1
ln -s /usr/local/ssl/lib/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1
```

<br/>
<br/>

#### 기존의 ssl 버전을 주로 사용하지 않을거라면 기존의 ssl 을 백업한 뒤 설치한 ssl의 버전으로 링크를 걸어줍니다.
> 이전 버전을 백업하는 경로는 프로젝트 성격에 따라 달라질 수 있으며 이전 버전을 사용하지 않는다면 삭제해도 무방합니다.
```
mv /usr/bin/openssl /usr/bin/openssl1.0.2
ln -s /usr/local/ssl/bin/openssl /usr/bin/openssl
```

<br/>
<br/>

#### 업데이트(설치)된 OpenSSL버전을 확인합니다.
```
openssl version
```

<br/>
<br/>

#### 만약 링크를 설정하지 않았다면 아래의 명령어로 확인할 수 있습니다.
```
/usr/local/ssl/bin/openssl version
```

<br/>
<br/>

#### OpenSSL 관련 가이드를 작성하기 위해서 해당 사이트를 참고 인용하였습니다.
- [Openssl 1.1.1d 업데이트](https://blanche-star.tistory.com/entry/APM-%EC%84%A4%EC%B9%98-openssl-%EC%B5%9C%EC%8B%A0%EB%B2%84%EC%A0%84%EC%84%A4%EC%B9%98%EC%86%8C%EC%8A%A4%EC%84%A4%EC%B9%98-shared%EC%84%A4%EC%B9%98)
