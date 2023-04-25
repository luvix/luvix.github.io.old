---
layout: post
title:  L2TP VPN server 만들기
description: SoftEtherVPN를 활용해서 간편하게 L2TP VPN 서버를 구축한다.
category: recipes
tags: [linux, VPN Server, SoftetherVPN, L2TP]
comments: true
image: 'https://user-images.githubusercontent.com/16158188/50405352-a8643700-07f6-11e9-94ea-6faab1183a4d.jpg'
---

SoftEther 는 자유 오픈소스, 크로스 플랫폼, 멀티 프로토콜 VPN 클라이언트이자 VPN 서버 소프트웨어다.
특히 OpenVPN 클라이언트로 매우 유명하며, VPN server 를 구축하기에도 매우 편리하다.
SoftEtherVPN으로 SSTP VPN server를 구축해본다.
마지막으로 windows 10 기본 VPN 클라이언트로 접속해본다.

> 본 문서는 DigitalOcean에서 제작한 [How to Setup a Multi-Protocol VPN Server Using SoftEther](https://www.digitalocean.com/community/tutorials/how-to-setup-a-multi-protocol-vpn-server-using-softether) 를 바탕으로 작성하였다.

## preperation

무릇 새로운 소프트웨어를 설치하려면, upgrade는 필수다.
그리고 설치에 필요한 package인 `build-essential` 를 다운받는다.

```
ubuntu@host:~$ sudo apt-get update && apt-get upgrade
ubuntu@host:~$ sudo apt-get install build-essential -y
```

## Download SoftEtherVPN package

SoftEther 다운로드페이지로 접속하여 설치 파일을 다운받아야 한다.
wget이나 curl로 받아도 되지만 여기서는 lynx를 사용하여 서버에서 직접 웹페이지로 접속하여 다운받는 방법을 설명한다.
lynx는 terminal 환경에서 웹페이지에 접속하는 도구다.

```
ubuntu@host:~$ sudo apt-get install lynx -y
```

lynx로 softether 다운로드 사이트에 접속한다.

```
ubuntu@host:~$ lynx http://www.softether-download.com/files/softether/
```

접속하면 아래와 같은 그림이 나타난다.

![lynx01](/postres/180527/lynx01.jpg)

맨 위는 가장 옛날, 맨 아래가 가장 최신(latest)다. 
따라서 가장 아래의 버전으로 ↓ 화살표 키보드를 눌러 내려간 후 엔터를 쳐서 고른다.

![lynx02](/postres/180527/lynx02.jpg)

우리는 linux 에서 작업하고 있이므로 linux를 고른다.

![lynx03](/postres/180527/lynx03.jpg)

VPN server를 구축하니까 server 선택.

![lynx04](/postres/180527/lynx04.jpg)

64bit 를 선택한다.

![lynx05](/postres/180527/lynx05.jpg)

초보 서버개발자는 lynx를 처음 써서 다운로드받는 방법을 모를 수 있다.
다운로드하려는 항목에 커서를 옮긴 후 'D'를 누르면 다운받아진다.

![lync06](/postres/180527/lynx06.jpg)

경로를 묻는데, 그냥 엔터 치면 Home으로 다운받는다.

## VPN 설치

> 주의! 여기서부터는 root 계정으로 진행한다.

vpn 서버는 root 계정으로 실행해야 한다.
하지만 우리는 일반 sudo 사용자로 실행해왔다.
따라서 root 계정으로 변경한다.

```
ubuntu@host:~$ sudo su
```

다운받은 폴더의 압축을 푼다.

```
root@host:/etc/vpnserver# tar xzvf softether-vpnserver-v4.27-9667-beta-2018.05.26-linux-x64-64bit.tar.gz
```

압축을 풀면 `vpnserver`라는 폴더가 생긴다.
이 폴더를 `/etc` 로 옮긴다.

```
root@host:/etc/vpnserver# mv vpnserver /etc
```

파일들 권한을 다듬어준다.

```
root@host:/etc/vpnserver# cd /etc/vpnserver/
root@host:/etc/vpnserver# chmod 600 *
root@host:/etc/vpnserver# chmod 700 vpnserver
root@host:/etc/vpnserver# chmod 700 vpncmd
```

Make한다.

```
root@host:/etc/vpnserver# make
```

이 때 여러가지 묻는다.
라이센스에 대한 질문이므로 모두 yes(1)를 선택한다.

## Service 등록

먼저 서비스 스크립트를 작성한다.

```
root@host:/etc/vpnserver# vi /etc/init.d/vpnserver
```

### /etc/init.d/vpnserver
``` sh
#! /bin/sh
# chkconfig: 2345 99 01
# Provides: SoftEtherVPN
# Required-Start: $remote_fs $syslog
# Required-Stop: $remote_fs $syslog
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: VPN server by SoftEtherVPN. This discription is from Example initscript.
# Description: This file should be used to construct scripts to be
# placed in /etc/init.d.
### END INIT INFO

DAEMON=/etc/vpnserver/vpnserver
LOCK=/var/lock/subsys/vpnserver
test -x $DAEMON || exit 0
case "$1" in
start)
$DAEMON start
touch $LOCK
;;
stop)
$DAEMON stop
rm $LOCK
;;
restart)
$DAEMON stop
sleep 3
$DAEMON start
;;
*)
echo "Usage: $0 {start|stop|restart}"
exit 1
esac
exit 0
~
```

만약 `/var/lock/subsys` 폴더가 없을 경우 그냥 만들면 된다.

여담인데, LSB 포맷으로 작성하려고 했는데 에러가 났다.
그래서 적당히 섞었다.

마지막으로 권한을 수정하고 서비스를 실행한다.
```
root@host:/etc/vpnserver# chmod 755 /etc/init.d/vpnserver
root@host:/etc/vpnserver# service vpnserver start
```

이왕 실행할꺼면 서버 켜질 때 실행되도록 _update.d_ 에 등록한다.

```
root@host:/etc/vpnserver# update-rc.d vpnserver defaults
```

## VPN Configuration

여기까지 그대로 왔다면 현재 경로가 `/usr/local/vpnserver`다.

여기서 `vpncmd`를 실행한다.

### 상태 확인

```
root@host:/etc/vpnserver# ./vpncmd
vpncmd command - SoftEther VPN Command Line Management Utility
SoftEther VPN Command Line Management Utility (vpncmd command)
Version 4.27 Build 9667   (English)
Compiled 2018/05/26 09:09:16 by yagi at pc33
Copyright (c) SoftEther VPN Project. All Rights Reserved.

By using vpncmd program, the following can be achieved.

1. Management of VPN Server or VPN Bridge
2. Management of VPN Client
3. Use of VPN Tools (certificate creation and Network Traffic Speed Test Tool)

Select 1, 2 or 3:
```

3 을 입력한 후 check를 입력한다.

```
Select 1, 2 or 3: 3

VPN Tools has been launched. By inputting HELP, you can view a list of the commands that can be used.

VPN Tools>
```

```
VPN Tools>check
Check command - Check whether SoftEther VPN Operation is Possible
---------------------------------------------------
SoftEther VPN Operation Environment Check Tool

Copyright (c) SoftEther VPN Project.
All Rights Reserved.

If this operation environment check tool is run on a system and that system passes, it is most likely that SoftEther VPN software can operate on that system. This ch

Checking 'Kernel System'...
              Pass
Checking 'Memory Operation System'...
              Pass
Checking 'ANSI / Unicode string processing system'...
              Pass
Checking 'File system'...
              Pass
Checking 'Thread processing system'...
              Pass
Checking 'Network system'...
              Pass

All checks passed. It is most likely that SoftEther VPN Server / Bridge can operate normally on this system.

The command completed successfully.

VPN Tools>
```

모두 Pass로 확인받아야 한다. 끝나면 `ctrl+D`로 나간다.

### admin 비밀번호 설정

vpncmd는 크게 세 개의 옵선이 있다.

1. VPN 서버를 설정한다. VPN 모드(NAT나 bridge)와  프로토콜 등을 설정한다.
2. 클라이언트를 설정한다. 여기서 말하는 클라이언트는 다른 VPN에 접속하는 도구다. 지금은 쓸 일 없다.
3. 세 번째는 인증과 네트워크를 설정한다.

조금 전에 check 명령으로 건강함을 확인했으니, 이제 admin 비밀번호를 설정한다.

```
root@host:/etc/vpnserver# ./vpncmd
vpncmd command - SoftEther VPN Command Line Management Utility
SoftEther VPN Command Line Management Utility (vpncmd command)
Version 4.27 Build 9667   (English)
Compiled 2018/05/26 09:09:16 by yagi at pc33
Copyright (c) SoftEther VPN Project. All Rights Reserved.

By using vpncmd program, the following can be achieved.

1. Management of VPN Server or VPN Bridge
2. Management of VPN Client
3. Use of VPN Tools (certificate creation and Network Traffic Speed Test Tool)

Select 1, 2 or 3: 1

Specify the host name or IP address of the computer that the destination VPN Server or VPN Bridge is operating on.
By specifying according to the format 'host name:port number', you can also specify the port number.
(When the port number is unspecified, 443 is used.)
If nothing is input and the Enter key is pressed, the connection will be made to the port number 8888 of localhost (this computer).
Hostname of IP Address of Destination: 
```

여기서 그냥 엔터 입력하면 자동을 IP를 잡아준다.
이렇게 자동으로 잡히는 IP는 외부에서 내 서버를 바라볼 때 보이는 IP다.

```
Hostname of IP Address of Destination: host

If connecting to the server by Virtual Hub Admin Mode, please input the Virtual Hub name.
If connecting by server admin mode, please press Enter without inputting anything.
Specify Virtual Hub Name:
Connection has been established with VPN Server "host" (port 443).

You have administrator privileges for the entire VPN Server.

VPN Server>ServerPasswordSet
ServerPasswordSet command - Set VPN Server Administrator Password
Please enter the password. To cancel press the Ctrl+D key.

Password: ****************************
Confirm input: ****************************


The command completed successfully.

VPN Server>
```

### Hub 만들기

다른 VPN server 도 그렇지만, 내부망을 다르게 가져갈 수 있다.
여기서 만드는 hub은 모두 같은 내부망을 가지...는걸로 알고 있다.
아주 확실하진 않으나 이런 이유 이외에는 없다고 생각한다.

```
VPN Server>HubCreate VPN
HubCreate command - Create New Virtual Hub
Please enter the password. To cancel press the Ctrl+D key.

Password: ****************************
Confirm input: ****************************


The command completed successfully.
```

### Hub 설정

먼저 hub로 접속한다.

```
VPN Server>Hub VPN
Hub command - Select Virtual Hub to Manage
The Virtual Hub "VPN" has been selected.
The command completed successfully.

VPN Server/VPN>
```

IP 할당하는 방법으로 `SecureNat` 을 선택한다.
다른 방법도 있지만 정신 건강 상 좋지 않다.

```
VPN Server/VPN>SecureNatEnable
SecureNatEnable command - Enable the Virtual NAT and DHCP Server Function (SecureNat Function)
The command completed successfully.

VPN Server/VPN>
```

계정을 만든다.
Group Name, User Full Name, User Description은 빈칸으로 두고 엔터를 입력해도 된다.

```
VPN Server/VPN>UserCreate luvix
UserCreate command - Create User
Assigned Group Name:

User Full Name:

User Description:

The command completed successfully.

VPN Server/VPN>
```

사용자 비밀번호를 등록한다.

```
VPN Server/VPN>UserPasswordSet luvix
UserPasswordSet command - Set Password Authentication for User Auth Type and Set Password
Please enter the password. To cancel press the Ctrl+D key.

Password: **********************
Confirm input: **********************


The command completed successfully.
```

### L2TP/IPSec 설정

```
VPN Server/VPN>IPsecEnable
IPsecEnable command - Enable or Disable IPsec VPN Server Function
Enable L2TP over IPsec Server Function (yes / no): 
```
물론 yes 다.

```
Enable L2TP over IPsec Server Function (yes / no): yes

Enable Raw L2TP Server Function (yes / no): 
```

물론 yes다.

```
Enable Raw L2TP Server Function (yes / no): yes

Enable EtherIP / L2TPv3 over IPsec Server Function (yes / no): 
```

물..아니, 이건 얘기가 좀 다르다.
이걸 설정하면 모바일에선 안 된다.
혹시 모르니까 no로 하자.

```
Enable EtherIP / L2TPv3 over IPsec Server Function (yes / no): no

Pre Shared Key for IPsec (Recommended: 9 letters at maximum): 
```

'IPSec 사전공유키', 혹은 '미리 공유한 키'를 여기서 등록한다.

```
Pre Shared Key for IPsec (Recommended: 9 letters at maximum): **************************

Default Virtual HUB in a case of omitting the HUB on the Username: VPN

The command completed successfully.

VPN Server/VPN>
```

참고로, 위의 코드를 보면 눈치챘겠지만, 이 설정은 Hub로 들어가지 않아도 된다.

## References

- [How to set up an SSTP Server](https://unix.stackexchange.com/a/76487)
- [DigitalOcean: How to Setup a Multi-Protocol VPN Server Using SoftEther](https://www.digitalocean.com/community/tutorials/how-to-setup-a-multi-protocol-vpn-server-using-softether)
