---
layout: post
title: yum을 쓰는 python 3.6 설치 스크립트
description: RHEL 운영체제에서 파이썬 3.6.1을 설치할 때 필요한 명령어들을 정리하였다.
category: recipes
tags: [snippet, python3, RHEL]
comments: true
image: 'https://user-images.githubusercontent.com/16158188/50405559-00e90380-07fa-11e9-8154-3e372afe6156.jpg'
---

python 3.6.1을 설치하는 명령어를 정리하였다.
AWS에서 실습을 진행하였으며, Amazon Linux x64 환경이다. RHEL(Centos, Redhat) 배포판에 대응한다.
다른 버전도 python 버전 번호만 바꾸면 동일하다.

## Commands

```
sudo yum -y gcc zlib-devel libffi-devel
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz
tar xzf Python-3.6.1.tgz
cd Python-3.6.1
./configure --prefix=/home/ubuntu/.local CXX="/usr/bin/g++"
./configure --prefix=/home/ubuntu/.local CXX="/usr/bin/g++" --enable-optimizations
make altinstall
rm Python-3.6.1.tgz
Python3.6 -V
```

## Comments

- gcc나 zlib가 있다면 첫번째 줄은 생략해도 된다. 하지만 잘 모르겠다면 일단 실행하는 게 좋다.  
- `make altinstall`는 기본 파이썬(`/usr/bin/python`)이 바뀌는 걸 방지해준다.  
- 30분 정도 걸린다. `sudo make altinstall`이 오래 걸린다.  
- 설치가 잘 진행되었다면 `python3.6 -V`의 결과가 `Python 3.6.1`로 나온다.  
- `openssl`어쩌구 저쩌구 에러가 날 경우 `libffi-devel`을 설치하면 된다.

## Script

다음 명령어를 그대로 복사해서 붙여넣어도 된다.

### sudo 권한이 없는 계정

```
curl -s https://raw.githubusercontent.com/luvix/snippet/master/py3/install-py3-yum.sh | bash
```

스크립트 다운로드가 끝나면 sudo 비밀번호를 물어본다.

### sudo 권한이 있는 계정

```
curl -s https://raw.githubusercontent.com/luvix/snippet/master/py3/install-py3-yum-sudoer.sh | bash
```

## References

- [How to Install Python 3.6.1 on CentOS/RHEL & Fedora](https://tecadmin.net/install-python-3-6-on-centos/#)
- [zipimport.ZipImportError: can't decompress data; zlib not available](http://unix.stackexchange.com/questions/291737/zipimport-zipimporterror-cant-decompress-data-zlib-not-available)
