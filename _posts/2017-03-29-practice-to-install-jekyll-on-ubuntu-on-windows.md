---
layout: post
title: Practice to install Jekyll on bash on Windows
description: bash on Windows 에 Jekyll을 설치해본다.
category: "Development Environment"
tags: [ssh, bash-on-windows]
comments: true
---
bash on Windows에 Jekyll을 설치해보았다.
rvm을 통해 Ruby를 설치한 후 jekyll을 설치하였다.

## Ruby 설치

### rvm 설치

stable 버전의 rvm을 설치한다.
중간중간 `Updating system{user} password required for`가 나타나면서 비밀번호를 입력하라고 요청한다.
계정의 비밀번호를 입력하면 된다. 이번 실습에선 두 번 요청하였다.
curl 앞에 backslach가 있는데, 빼먹지 않도록 주의한다. 버전 충돌을 막아준다.

```
theo@Bash-On-Windows:/mnt/c/Users/theodore$ \curl -L https://get.rvm.io | bash -s stable --ruby
```

### rvm 적용

rvm을 bash에 등록시킨다. 등록하지 않으면 rvm 명령을 실행할 수 없다.

{% highlight console %}
theo@Bash-On-Windows:/mnt/c/Users/theodore$ rvm install ruby-2.4.1
'rvm' 명령은 찾을 수 없지만 비슷한  '20' 명령이 있습니다.
rvm: 명령을 찾을 수 없습니다
theo@Bash-On-Windows:/mnt/c/Users/theodore$ source /home/theo/.rvm/scripts/rvm
theo@Bash-On-Windows:/mnt/c/Users/theodore$
{% endhighlight %}

### ruby 2.4.1 설치

버전명을 주의한다.

```
theo@Bash-On-Windows:/mnt/c/Users/theodore$ rvm install ruby-2.4.1
```

### default ruby를 2.4.1로 변경

```
theo@Bash-On-Windows:/mnt/c/Users/theodore$ rvm --default use ruby-2.4.1
```

### Jekyll 설치

```
theo@Bash-On-Windows:/mnt/c/Users/theodore$ gem install jekyll
```

총 18개의 gem이 설치된다.
그 목록은 다음과 같다.

### bundler 및 bundle 설치

{% highlight console %}
theo@Bash-On-Windows:/mnt/c/Users/theodore$ gem install bundle
{% endhighlight %}
bundle을 설치할 때 bundler도 같이 설치된다.


## Jekyll 프로젝트 설정

먼저 jekyll 프로젝트가 있는 디렉토리로 이동한다.

### Jekyll 프로젝트에 bundle설치

```
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ bundle install
```

> **주의!** git이 설치되어 있지 않다면 Git를 설치한다.
bundle이 Git 설정을 읽어야하기 때문이다.
실습에선 Git이 없어서 Git 설치과정도 진행하였다.

### Git 설치

```
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ sudo apt-get install git
```

### 다시 Jekyll 프로젝트에 bundle설치

```
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ bundle install
```
20개의 gem이 새로 설치되고 3개의 gem이 업데이트된다.

### Jekyll 테스트

```
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ bundle exec jekyll serve --detach
```

실하면 다음과 같이 로그가 출력된다.

```
Configuration file: /mnt/c/theodore-kim.github.io/_config.yml
            Source: /mnt/c/theodore-kim.github.io
       Destination: /mnt/c/theodore-kim.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 1.524 seconds.
Auto-regeneration: disabled when running server detached.
Configuration file: /mnt/c/theodore-kim.github.io/_config.yml
    Server address: http://127.0.0.1:4000/
Server detached with pid '9422'. Run `pkill -f jekyll' or `kill > -9 9422' to stop the server.
```

이 로그에서 다음을 확인해야 한다.

1. 어느 파일에서 config를 읽는가(여기서는 `theodore-kim.github.io/_config.yml`에서 읽고 있다는 걸 알 수 있다.)
1. 서버 pid를 확인하거나 jekyll proc을 끄는 방법.
    - 개인적으로 pid를 확인하길 바란다. `pkill`보다는 `kill`을 더 많이 쓰기 때문이다.

이로써 localhost에 포트 4000으로 jekyll에 접속할 수 있게되었다.

### 참고: Jekyll 다시 실행하기

bash on Windows의 버전이 낮을 경우 reload가 불가능할 수 있다.
실습에선 잘 실행되었다.
또한 Jekyll이 git 관련 에러를 출력하지만, 재실행에는 문제가 없다.

```
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ jekyll serve --watch
```

## 내일 다시 시작하기

다시 시작할 때는 source 명령어로 rvm을 불러온 후 bundle과 jekyll 을 차례대로 실행하면 된다.

{% highlight console %}
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ source /home/theo/.rvm/scripts/rvm
theo@Bash-On-Windows:/mnt/c/theodore-kim.github.io$ bundle exec jekyll serve --detach
{% endhighlight %}


## 정리

rvm을 이용하여 ruby를 설치한 후, jekyll과 필요한 패키지들을 설치하였다.
추가적으로 git도 설치한 후 jekyll 프로젝트에 bundle을 설치하였다.
처음 설치할 때 사용된 스크립트들은 다음과 같다.

```
\curl -L https://get.rvm.io | bash -s stable --ruby
source /home/theo/.rvm/scripts/rvm
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
source /home/theo/.rvm/scripts/rvm
bundle exec jekyll serve --detach
jekyll serve --watch
```

## References

- [install ruby](http://railsapps.github.io/install-ruby.html)
- [how to install jekyll on centos 7](https://hostpresto.com/community/tutorials/how-to-install-jekyll-on-centos-7/)
