---
layout: post
title:  Practice to run Jenkins on Docker in synology NAS with SSL certificated by Let's Encrypt
category: "System Engineering"
tags: [practice, synology, jenkins, docker, ssl, https]
comments: true
---

# 경고
## 재부팅하면 다시 복구할 수 없습니다. (시놀로지의 도커 패키지 이슈)
## CANNOT RESTART WHEN YOUR NAS REBOOT. (Docker package for Synology NAS issue)

> 공식적으로 젠킨스가 도커를 지원하면서 도커를 설치할 수 있는 시놀로지 나스에 설치할 수 있게되었다.
만약 DNS와 Let's Encrypt등을 통해 소켓계층보안(SSL) 설정까지 완료한 사용자라면 당연히 젠킨스도 보안 연결을 해야 한다.
하지만 기본적으로 공식 젠킨스 도커 배포사이트에는 HTTP 연결만 안내되어 있고, HTTPS 연결은 '...이런 식으로 하면 될 겁니다. 참 쉽죠?' 수준의 설명만 나와있다.
그래서 실제로 나스에 SSL 설정까지 적용하여 설치하는 방법에 대해 살펴보았다.  
먼저 나스에 기본으로 설정된 인증서를 확인한 후 젠킨스용 인증서를 준비했다.
그리고 Docker on Jenkins가 지원하는 운영 설정을 활용하여 기본 인증서를 연결하였다.
마지막으로 실제 운영을 위한 추가 설정이 포함된 환경 설정을 위자드에 추가하여 container를 운영시켰다.
이를 통해 네트워크상의 보안성 확보하였을 뿐만 아니라 기본 인증서가 변경되어도 container 재시작만으로 인증서를 적용시킬 수 있게 되었다.


## 1. Let's Encrypt의 certification이 default certification인지 확인하기
일단, 나스에서 Let's Encrypt 인증서가 기본(default)으로 설정되어 있는 지 확인한다.

![Let's Encrypt에서 발급받은 인증서가 적용된 시놀로지 나스]({{site.url}}/resources/synology_lets_encrypt_certification-170328.jpg)

이렇게 설정되어 있다면 해당 인증서가 생성한 키들이<code>/usr/syno/etc/certificate/system/default</code>에 있다.
SSH를 통해 나스 콘솔로 접속하여 해당 위치로 디렉토리를 옮겨보면 여러 pem이 있는 것을 알 수 있다.

{% highlight console %}
$ ssh theodore@diskstation
theodore@diskstation:~$ cd /usr/syno/etc/certificate/system/default
theodore@diskstation:/usr/syno/etc/certificate/system/default$ ls -al
total 36
drwxr-xr-x 2 root root 4096 Jan 28 16:05 .
drwxr-xr-x 3 root root 4096 Jan 11 23:17 ..
-r-------- 1 root root 1809 Jan 11 23:17 cert.pem
-r-------- 1 root root 1647 Jan 11 23:17 chain.pem
-r-------- 1 root root 3456 Jan 11 23:17 fullchain.pem
-r-------- 1 root root 1679 Jan 11 23:17 privkey.pem
theodore@diskstation:/usr/syno/etc/certificate/system/default$
{% endhighlight %}

## 2. Jenkins on Docker용 certification 준비하기
Docker가 사용할 수 있는 cert와 privkey를 만든다.
어떠한 방식으로 만들었던 나스가 기본적으로 사용하는 인증서들은 모두 root 소유다.
하지만 Docker 계정은 root도 아니고 root 그룹도 아니다.
따라서 Docker 계정이 소유권을 가지는 인증서를 준비해야 한다.

먼저 cert.pem와 privkey.pem를 복사한다.
이름은 간단하게 letsencrypt.crt와 letsencrypt.key로 변경하였다.
{% highlight console %}

{% endhighlight %}

그리고 권한을 변경하였다.
{% highlight console %}

{% endhighlight %}

## 3. Jenkins on Docker container 설정하기
도커 상의 젠킨스에 소켓계층보안 옵션을 추가하려면 https옵션을 활성화 한 후 cert와 private key의 위치를 지정해주면 된다.
경우에 따라 포트도 함께 바꿀 수 있다. 여기서는 다음의 옵션을 사용했다.

### 포트 변경(-p)

### 데몬으로 실행(-d)

### JENKINS_OPTS 환경 추가(--env)
- http 비활성화 및 https 활성화
- cert 파일 위치 지정

### privkey 파일 위치 지정

### Jenkins 이미지 선택
현재 stable버전인 2.32.3을 선택하였다.

## 4. Jenkins on Docker container 운영하기
3번에서 만든 코드를 콘솔에 직접 입력해도 된다.
하지만 우린 나스에서 운영하고, 설치 마지막 과정에 필요한 코드는 나스에서 확인하는게 편하므로 나스에서 제공하는 도커 인터페이스를 활용하였다.
이제 접속하면 인증코드를 입력하라는 페이지가 암호화된 네트워크를 통해 우리 앞에 나타난 걸 확인할 수 있다.
인증코드는 나스의 도커에서 세부사항의 로그 항목을 통해 확인할 수 있다.

## 5. 정리


## Reference
- [Jenkins Official Docker Repository, Jenkins.io](https://hub.Docker.com/_/Jenkins/)  
- [Synology Gitlab Setup SSL over Let’s Encrypt, chpresearch](https://chpresearch.wordpress.com/2016/10/04/synology-gitlab-setup-ssl-over-lets-encrypt/)
