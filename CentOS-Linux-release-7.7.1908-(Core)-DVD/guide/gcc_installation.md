# GCC
> CentOS 7 에서 GCC를 설치하면 4.8.5 버전이 설치 됩니다.   
해당 가이드에서는 GCC, G++ 7.x 버전을 설치 및 함께 사용하는 방법이 기재되어 있습니다.

<br/>
<br/>

#### SCL 및 devtoolset-7 을 다운로드 합니다.
```
sudo yum install centos-release-scl
sudo yum install devtoolset-7
```

<br/>
<br/>

#### devtoolset-7 을 활성화 합니다.
```
scl enable devtoolset-7 bash
```

<br/>
<br/>

#### gcc 및 g++ 이 정상적으로 활성화 되었는지 확인합니다.
```
gcc --version
g++ --version
```

<br/>
<br/>
