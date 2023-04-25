---
layout: post
title: winbash로 jekyll 실행하기
description: Windows에서 Windows Subsystem for Linux로 shell script를 실행한다.
category: recipes
tags: [win10, winbash, ruby, jekyll]
comments: true
---
윈도우 10부터는 ubuntu를 설치할 수 있으면서(비록 개발자 모드에서만 가능하지만) bash도 실행할 수 있다.
본 실습에서는 Windows에서 Windows Subsystem for Linux로 shell scripy를 실행해본다.

### rvm 설치

stable 버전의 rvm을 설치한다.
중간중간 `Updating system{user} password required for`가 나타나면서 비밀번호를 입력하라고 요청한다.
계정의 비밀번호를 입력하면 된다. 이번 실습에선 두 번 요청하였다.  
curl 앞에 backslach가 있는데, 빼먹지 않도록 주의한다. 버전 충돌을 막아준다.

```
luvix@winbash:/mnt/c/Users/luvix$ \curl -L https://get.rvm.io | bash -s stable --ruby
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   194  100   194    0     0    179      0  0:00:01  0:00:01 --:--:--   179
100 24062  100 24062    0     0  15327      0  0:00:01  0:00:01 --:--:-- 15327
Downloading https://github.com/rvm/rvm/archive/1.29.1.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.1/1.29.1.tar.gz.asc
Found PGP signature at: 'https://github.com/rvm/rvm/releases/download/1.29.1/1.29.1.tar.gz.asc',
but no GPG software exists to validate it, skipping.

Installing RVM to /home/luvix/.rvm/
    Adding rvm PATH line to /home/luvix/.profile /home/luvix/.mkshrc /home/luvix/.bashrc /home/luvix/.zshrc.
    Adding rvm loading line to /home/luvix/.profile /home/luvix/.bash_profile /home/luvix/.zlogin.
Installation of RVM in /home/luvix/.rvm/ is almost complete:

  * To start using RVM you need to run `source /home/luvix/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.

# luvix,
#
#   Thank you for using RVM!
#   We sincerely hope that RVM helps to make your life easier and more enjoyable!!!
#
# ~Wayne, Michal & team.

In case of problems: https://rvm.io/help and https://twitter.com/rvm_io
rvm 1.29.1 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io/]
Searching for binary rubies, this might take some time.
Found remote file https://rubies.travis-ci.org/ubuntu/14.04/x86_64/ruby-2.4.0.tar.bz2
Checking requirements for ubuntu.
Installing requirements for ubuntu.
Updating systemluvix password required for 'apt-get --quiet --yes update': ..\
......
eInstalling required packages: g++, gcc, libssl-dev, make, libc6-dev, zlib1g-dev, libyaml-dev, libsqlite3-dev, sqlite3, autoconf, libgmp-dev, libgdbm-dev, libncurses5-dev, automake, libtool, bison, pkg-config, libffi-dev, libgmp-dev, libreadline6-devluvix password required for 'apt-get --no-install-recommends --yes install g++ gcc libssl-dev make libc6-dev zlib1g-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgmp-dev libgdbm-dev libncurses5-dev automake libtool bison pkg-config libffi-dev libgmp-dev libreadline6-dev': ..\
luvix password required for 'apt-get --no-install-recommends --yes install g++ gcc libssl-dev make libc6-dev zlib1g-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgmp-dev libgdbm-dev libncurses5-dev automake libtool bison pkg-config libffi-dev libgmp-dev libreadline6-dev':
..................................................
Requirements installation successful.
ruby-2.4.0 - #configure
ruby-2.4.0 - #download
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:29 --:--:--     0
100 14.0M  100 14.0M    0     0   364k      0  0:00:39  0:00:39 --:--:-- 3317k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.4.0 - #validate archive
ruby-2.4.0 - #extract
ruby-2.4.0 - #validate binary
ruby-2.4.0 - #setup
ruby-2.4.0 - #gemset created /home/luvix/.rvm/gems/ruby-2.4.0@global
ruby-2.4.0 - #importing gemset /home/luvix/.rvm/gemsets/global.gems
ruby-2.4.0 - #generating global wrappers........
ruby-2.4.0 - #gemset created /home/luvix/.rvm/gems/ruby-2.4.0
ruby-2.4.0 - #importing gemsetfile /home/luvix/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.4.0 - #generating default wrappers........
Creating alias default for ruby-2.4.0...

  * To start using RVM you need to run `source /home/luvix/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
luvix@winbash:/mnt/c/Users/luvix$
```

### rvm 적용

rvm을 bash에 등록시킨다. 등록하지 않으면 rvm 명령을 실행할 수 없다.

```
luvix@winbash:/mnt/c/Users/luvix$ rvm install ruby-2.4.1
'rvm' 명령은 찾을 수 없지만 비슷한  '20' 명령이 있습니다.
rvm: 명령을 찾을 수 없습니다
luvix@winbash:/mnt/c/Users/luvix$ source /home/luvix/.rvm/scripts/rvm
luvix@winbash:/mnt/c/Users/luvix$
```

### ruby 2.4.1 설치

버전명을 주의한다.


    luvix@winbash:/mnt/c/Users/luvix$ rvm install ruby-2.4.1
    Searching for binary rubies, this might take some time.
    Found remote file https://rubies.travis-ci.org/ubuntu/14.04/x86_64/ruby-2.4.1.tar.bz2
    Checking requirements for ubuntu.
    Requirements installation successful.
    ruby-2.4.1 - #configure
    ruby-2.4.1 - #download
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
    100 14.1M  100 14.1M    0     0   227k      0  0:01:03  0:01:03 --:--:--  327k
    No checksum for downloaded archive, recording checksum in user configuration.
    ruby-2.4.1 - #validate archive
    ruby-2.4.1 - #extract
    ruby-2.4.1 - #validate binary
    ruby-2.4.1 - #setup
    ruby-2.4.1 - #gemset created /home/luvix/.rvm/gems/ruby-2.4.1@global
    ruby-2.4.1 - #importing gemset /home/luvix/.rvm/gemsets/global.gems...................................
    ruby-2.4.1 - #generating global wrappers........
    ruby-2.4.1 - #gemset created /home/luvix/.rvm/gems/ruby-2.4.1
    ruby-2.4.1 - #importing gemsetfile /home/luvix/.rvm/gemsets/default.gems evaluated to empty gem list
    ruby-2.4.1 - #generating default wrappers........
    luvix@winbash:/mnt/c/Users/luvix$


### default ruby를 2.4.1로 변경

```
luvix@winbash:/mnt/c/Users/luvix$ rvm --default use ruby-2.4.1
Using /home/luvix/.rvm/gems/ruby-2.4.1
luvix@winbash:/mnt/c/Users/luvix$
```

### Jekyll 설치

    luvix@winbash:/mnt/c/Users/luvix$ gem install jekyll
    Fetching: public_suffix-2.0.5.gem (100%)
    Successfully installed public_suffix-2.0.5
    Fetching: addressable-2.5.0.gem (100%)
    Successfully installed addressable-2.5.0
    Fetching: colorator-1.1.0.gem (100%)
    Successfully installed colorator-1.1.0
    Fetching: sass-3.4.23.gem (100%)
    Successfully installed sass-3.4.23
    Fetching: jekyll-sass-converter-1.5.0.gem (100%)
    Successfully installed jekyll-sass-converter-1.5.0
    Fetching: rb-fsevent-0.9.8.gem (100%)
    Successfully installed rb-fsevent-0.9.8
    Fetching: ffi-1.9.18.gem (100%)
    Building native extensions.  This could take a while...
    /home/luvix/.rvm/rubies/ruby-2.4.1/lib/ruby/2.4.0/rubygems/ext/builder.rb:76: warning: Insecure world writable dir /home/luvix/.rvm/gems/ruby-2.4.1/bin in PATH, mode 040777
    Successfully installed ffi-1.9.18
    Fetching: rb-inotify-0.9.8.gem (100%)
    Successfully installed rb-inotify-0.9.8
    Fetching: listen-3.0.8.gem (100%)
    Successfully installed listen-3.0.8
    Fetching: jekyll-watch-1.5.0.gem (100%)
    Successfully installed jekyll-watch-1.5.0
    Fetching: kramdown-1.13.2.gem (100%)
    Successfully installed kramdown-1.13.2
    Fetching: liquid-3.0.6.gem (100%)
    Successfully installed liquid-3.0.6
    Fetching: mercenary-0.3.6.gem (100%)
    Successfully installed mercenary-0.3.6
    Fetching: forwardable-extended-2.6.0.gem (100%)
    Successfully installed forwardable-extended-2.6.0
    Fetching: pathutil-0.14.0.gem (100%)
    Successfully installed pathutil-0.14.0
    Fetching: rouge-1.11.1.gem (100%)
    Successfully installed rouge-1.11.1
    Fetching: safe_yaml-1.0.4.gem (100%)
    Successfully installed safe_yaml-1.0.4
    Fetching: jekyll-3.4.3.gem (100%)
    Successfully installed jekyll-3.4.3
    Parsing documentation for public_suffix-2.0.5
    Installing ri documentation for public_suffix-2.0.5
    Parsing documentation for addressable-2.5.0
    Installing ri documentation for addressable-2.5.0
    Parsing documentation for colorator-1.1.0
    Installing ri documentation for colorator-1.1.0
    Parsing documentation for sass-3.4.23
    Installing ri documentation for sass-3.4.23
    Parsing documentation for jekyll-sass-converter-1.5.0
    Installing ri documentation for jekyll-sass-converter-1.5.0
    Parsing documentation for rb-fsevent-0.9.8
    Installing ri documentation for rb-fsevent-0.9.8
    Parsing documentation for ffi-1.9.18
    Installing ri documentation for ffi-1.9.18
    Parsing documentation for rb-inotify-0.9.8
    Installing ri documentation for rb-inotify-0.9.8
    Parsing documentation for listen-3.0.8
    Installing ri documentation for listen-3.0.8
    Parsing documentation for jekyll-watch-1.5.0
    Installing ri documentation for jekyll-watch-1.5.0
    Parsing documentation for kramdown-1.13.2
    Installing ri documentation for kramdown-1.13.2
    Parsing documentation for liquid-3.0.6
    Installing ri documentation for liquid-3.0.6
    Parsing documentation for mercenary-0.3.6
    Installing ri documentation for mercenary-0.3.6
    Parsing documentation for forwardable-extended-2.6.0
    Installing ri documentation for forwardable-extended-2.6.0
    Parsing documentation for pathutil-0.14.0
    Installing ri documentation for pathutil-0.14.0
    Parsing documentation for rouge-1.11.1
    Installing ri documentation for rouge-1.11.1
    Parsing documentation for safe_yaml-1.0.4
    Installing ri documentation for safe_yaml-1.0.4
    Parsing documentation for jekyll-3.4.3
    Installing ri documentation for jekyll-3.4.3
    Done installing documentation for public_suffix, addressable, colorator, sass, jekyll-sass-converter, rb-fsevent, ffi, rb-inotify, listen, jekyll-watch, kramdown, liquid, mercenary, forwardable-extended, pathutil, rouge, safe_yaml, jekyll after 34 seconds
    18 gems installed
    luvix@winbash:/mnt/c/Users/luvix$


### bundler 및 bundle 설치

bundle을 설치할 때 bundler도 같이 설치된다.

```
luvix@winbash:/mnt/c/Users/luvix$ gem install bundle
Fetching: bundler-1.14.6.gem (100%)
Successfully installed bundler-1.14.6
Fetching: bundle-0.0.1.gem (100%)
Successfully installed bundle-0.0.1
Parsing documentation for bundler-1.14.6
Installing ri documentation for bundler-1.14.6
Parsing documentation for bundle-0.0.1
Installing ri documentation for bundle-0.0.1
Done installing documentation for bundler, bundle after 8 seconds
2 gems installed
luvix@winbash:/mnt/c/Users/luvix$
```

## Jekyll 프로젝트 설정

먼저 jekyll 프로젝트가 있는 디렉토리로 이동한다.

```
luvix@winbash:/mnt/c/Users/luvix$ cd /mnt/c/luvix.github.io/
luvix@winbash:/mnt/c/luvix.github.io$
```

### Jekyll 프로젝트에 bundle설치

```
luvix@winbash:/mnt/c/luvix.github.io$ bundle install

[!] There was an error parsing `Gemfile`:
[!] There was an error while loading `forty_jekyll_theme.gemspec`: No such file or directory - git. Bundler cannot continue.

 #  from /mnt/c/luvix.github.io/forty_jekyll_theme.gemspec:13
 #  -------------------------------------------
 #
 >    spec.files         = `git ls-files -z`.split("\x0").select { |f| f.match(%r{^(assets|_layouts|_includes|_sass|LICENSE|README)}i) }
 #
 #  -------------------------------------------
. Bundler cannot continue.

 #  from /mnt/c/luvix.github.io/Gemfile:2
 #  -------------------------------------------
 #  source "https://rubygems.org"
 >  gemspec
 #  -------------------------------------------
 luvix@winbash:/mnt/c/luvix.github.io$
```

**주의!** git이 설치되어 있지 않다면 Git를 설치한다.
bundle이 Git 설정을 읽어야하기 때문이다.  
실습에선 Git이 없어서 Git 설치과정도 진행하였다.

### Git 설치


    luvix@winbash:/mnt/c/luvix.github.io$ sudo apt-get install git
    sudo: unable to resolve host winbash
    [sudo] password for luvix:
    패키지 목록을 읽는 중입니다... 완료0%
    의존성 트리를 만드는 중입니다
    상태 정보를 읽는 중입니다... 완료
    다음 패키지가 자동으로 설치되었지만 더 이상 필요하지 않습니다:
      libfreetype6 os-prober
    Use 'apt-get autoremove' to remove them.
    다음 패키지를 더 설치할 것입니다:
      git-man liberror-perl
    제안하는 패키지:
      git-daemon-run git-daemon-sysvinit git-doc git-el git-email git-gui gitk
      gitweb git-arch git-bzr git-cvs git-mediawiki git-svn
    다음 새 패키지를 설치할 것입니다:
      git git-man liberror-perl
    0개 업그레이드, 3개 새로 설치, 0개 제거 및 42개 업그레이드 안 함.
    3,063 k바이트 아카이브를 받아야 합니다.
    이 작업 후 21.9 M바이트의 디스크 공간을 더 사용하게 됩니다.
    계속 하시겠습니까? [Y/n] Y
    받기:1 http://archive.ubuntu.com/ubuntu/ trusty/main liberror-perl all 0.17-1.1 [21.1 kB]
    받기:2 http://archive.ubuntu.com/ubuntu/ trusty-updates/main git-man all 1:1.9.1-1ubuntu0.4 [699 kB]
    받기:3 http://archive.ubuntu.com/ubuntu/ trusty-updates/main git amd64 1:1.9.1-1ubuntu0.4 [2,343 kB]
    내려받기 3,063 k바이트, 소요시간 15초 (202 k바이트/초)
    Selecting previously unselected package liberror-perl.
    (데이터베이스 읽는중 ...현재 28343개의 파일과 디렉터리가 설치되어 있습니다. )
    Preparing to unpack .../liberror-perl_0.17-1.1_all.deb ...
    Unpacking liberror-perl (0.17-1.1) ...
    Selecting previously unselected package git-man.
    Preparing to unpack .../git-man_1%3a1.9.1-1ubuntu0.4_all.deb ...
    Unpacking git-man (1:1.9.1-1ubuntu0.4) ...
    Selecting previously unselected package git.
    Preparing to unpack .../git_1%3a1.9.1-1ubuntu0.4_amd64.deb ...
    Unpacking git (1:1.9.1-1ubuntu0.4) ...
    Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
    liberror-perl (0.17-1.1) 설정하는 중입니다 ...
    git-man (1:1.9.1-1ubuntu0.4) 설정하는 중입니다 ...
    git (1:1.9.1-1ubuntu0.4) 설정하는 중입니다 ...
    luvix@winbash:/mnt/c/luvix.github.io$


### 다시 Jekyll 프로젝트에 bundle설치


    luvix@winbash:/mnt/c/luvix.github.io$ bundle install
    Fetching gem metadata from https://rubygems.org/...........
    Fetching version metadata from https://rubygems.org/..
    Fetching dependency metadata from https://rubygems.org/.
    Using public_suffix 2.0.5
    Using colorator 1.1.0
    Using ffi 1.9.18
    Using forty_jekyll_theme 1.2 from source at `.`
    Using forwardable-extended 2.6.0
    Using sass 3.4.23
    Using rb-fsevent 0.9.8
    Using kramdown 1.13.2
    Using liquid 3.0.6
    Using mercenary 0.3.6
    Using rouge 1.11.1
    Using safe_yaml 1.0.4
    Using bundler 1.14.6
    Using addressable 2.5.0
    Using rb-inotify 0.9.8
    Using pathutil 0.14.0
    Using jekyll-sass-converter 1.5.0
    Using listen 3.0.8
    Using jekyll-watch 1.5.0
    Installing jekyll 3.4.2
    Bundle complete! 3 Gemfile dependencies, 20 gems now installed.
    Use `bundle show [gemname]` to see where a bundled gem is installed.
    luvix@winbash:/mnt/c/luvix.github.io$


### Jekyll 테스트

    luvix@winbash:/mnt/c/luvix.github.io$ bundle exec jekyll serve --detach
    Configuration file: /mnt/c/luvix.github.io/_config.yml
    Configuration file: /mnt/c/luvix.github.io/_config.yml
                Source: /mnt/c/luvix.github.io
          Destination: /mnt/c/luvix.github.io/_site
    Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 1.524 seconds.
    Auto-regeneration: disabled when running server detached.
    Configuration file: /mnt/c/luvix.github.io/_config.yml
        Server address: http://127.0.0.1:4000/
    Server detached with pid '9422'. Run `pkill -f jekyll' or `kill -9 9422' to stop the server.
    luvix@winbash:/mnt/c/luvix.github.io$

이로써 localhost에 포트 4000으로 jekyll에 접속할 수 있게되었다.

### 참고: Jekyll 다시 실행하기

Bash on Windows의 버전이 낮을 경우 reload가 불가능할 수 있다.
실습에선 잘 실행되었다.  
또한 Jekyll이 git 관련 에러를 출력하지만, 재실행에는 문제가 없다.


    luvix@winbash:/mnt/c/luvix.github.io$ jekyll serve --watch
    Configuration file: /mnt/c/luvix.github.io/_config.yml
    Configuration file: /mnt/c/luvix.github.io/_config.yml
                Source: /mnt/c/luvix.github.io
          Destination: /mnt/c/luvix.github.io/_site
    Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 0.623 seconds.
                        Auto-regeneration may not work on some Windows versions.
                        Please see: https://github.com/Microsoft/BashOnWindows/issues/216
                        If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch.
    jekyll 3.4.2 | Error:  Invalid argument - Failed to watch "/mnt/c/luvix.github.io/.git/hooks": the given event mask contains no legal events; or fd is not an inotify file descriptor.
    luvix@winbash:/mnt/c/luvix.github.io$


## 내일 다시 시작하기

다시 시작할 때는 source 명령어로 rvm을 불러온 후 bundle과 jekyll 을 차례대로 실행하면 된다.

    luvix@winbash:/mnt/c/luvix.github.io$ source /home/luvix/.rvm/scripts/rvm
    luvix@winbash:/mnt/c/luvix.github.io$ bundle exec jekyll serve --detach
    Configuration file: /mnt/d/Documents/Codeworks/luvix/luvix.github.io/_config.yml
    Configuration file: /mnt/d/Documents/Codeworks/luvix/luvix.github.io/_config.yml
                Source: /mnt/d/Documents/Codeworks/luvix/luvix.github.io
          Destination: /mnt/d/Documents/Codeworks/luvix/luvix.github.io/_site
    Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 0.634 seconds.
    Auto-regeneration: disabled when running server detached.
    Configuration file: /mnt/d/Documents/Codeworks/luvix/luvix.github.io/_config.yml
        Server address: http://127.0.0.1:4000/
    Server detached with pid '99'. Run `pkill -f jekyll' or `kill -9 99' to stop the server.
    luvix@winbash:/mnt/c/luvix.github.io$ [2017-03-31 22:42:48] ERROR `/favicon.ico' not found.


## 정리

rvm을 이용하여 ruby를 설치한 후, jekyll과 필요한 패키지들을 설치하였다.
추가적으로 git도 설치한 후 jekyll 프로젝트에 bundle을 설치하였다.  
처음 설치할 때 사용된 스크립트들은 다음과 같다.

``` 
\curl -L https://get.rvm.io | bash -s stable --ruby
source /home/luvix/.rvm/scripts/rvm
rvm install ruby-2.4.1
rvm --default use ruby-2.4.1
gem install jekyll
gem install bundle
sudo apt-get -y install git
```

디렉토리를 jekyll 프로젝트로 옮긴 후에 사용된 스크립트는 다음과 같다.

``` 
bundle install
bundle exec jekyll serve --detach
jekyll serve --watch
```

다시 시작할 때 사용된 스크립트는 다음과 같다.

``` 
source /home/luvix/.rvm/scripts/rvm
bundle exec jekyll serve --detach
jekyll serve --watch
```

## References

- [install ruby](http://railsapps.github.io/install-ruby.html)
- [how to install jekyll on centos 7](https://hostpresto.com/community/tutorials/how-to-install-jekyll-on-centos-7/)
