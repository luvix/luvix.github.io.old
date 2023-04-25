---
layout: post
title: apt를 쓰는 python 3.6 설치 스크립트
description: Ubuntu 운영체제에서 파이썬 3.6.1을 설치할 때 필요한 명령어들을 정리하였다.
category: recipes
tags: [snippet, python3, ubuntu]
comments: true
image: 'https://user-images.githubusercontent.com/16158188/50405566-0f371f80-07fa-11e9-8c91-996cbaf8fe18.jpg'
---

파이썬 3.6.1을 설치하는 명령어를 정리하였다.
AWS에서 실습을 진행하였으며, Ubuntu 16.04 LTS x64 환경이다.
다른 버전도 버전 번호만 바꾸면 동일하다.

## Commands

``` cmd
sudo apt install build-essential checkinstall openssl
sudo apt-get install -y libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz
tar xvf Python-3.6.1.tar.xz
cd Python-3.6.1
./configure --prefix=/home/ubuntu/.local CXX="/usr/bin/g++"
./configure --prefix=/home/ubuntu/.local CXX="/usr/bin/g++" --enable-optimizations
make altinstall
source ~/.profile
rm Python-3.6.1.tar.xz
python3.6 -V
```

## Comments

- 설치해야되는 시스템 패키지별로 설치 방법이 다르다. 어떤 건 `apt install` 이고 어떤 건 `apt-get install` 이다. 이 점을 주의한다.
- `configure`에서 사용하는 옵션 중 `--enable-optimizations`는 한 번 실행한 후에 적용 가능하다.
- `CXX` 옵션은 C++로 빌드할 때 필요한 컴파일러를 지정한다.
그리고 linux는 보통 `g++`가 설치되어 있다.
그런데 종종 이 경로가 달라서 C++로 빌드할 수 없을 때가 있으므로 지정해주자.
`g++`의 경로를 알고 싶다면 `which g++`를 입력해서 확인한다.
지정 안 해도 설치는 된다.
- 30분 정도 걸린다.
- 이 방법을 통해 설치한 python의 경로는 `/home/ubuntu/.local/bin`(줄여서 쓰면 `~/.local/bin`)이다.
이 경로는 '~/.profile'에 이미 등록되어 있으므로 source를 사용하여 다시 로드해주기만 하면 된다.
참고로 aws용 ubuntu는 bash가 `~/.profile`이지만 일반적으로는 `~/.bash_profile`이다.

## Script

다음 명령어를 그대로 복사해서 붙여넣어도 된다.

### sudo 권한이 없는 계정

```
curl -s https://raw.githubusercontent.com/luvix/snippet/master/py3/install-py3-apt.sh | bash
```

스크립트 다운로드가 끝나면 sudo 비밀번호를 물어본다.

### sudo 권한이 있는 계정

```
curl -s https://raw.githubusercontent.com/luvix/snippet/master/py3/install-py3-apt-sudoer.sh | bash
```

## References

- [How to Install Python 3.6 on Ubuntu 16.04, Ubuntu 16.10, 17.04](https://www.linuxbabe.com/ubuntu/install-python-3-6-ubuntu-16-04-16-10-17-04)
- [Python ./configure does not find g++ compiler](https://superuser.com/questions/787412/python-configure-does-not-find-g-compiler)
