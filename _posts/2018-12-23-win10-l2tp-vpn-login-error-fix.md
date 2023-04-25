---
layout: post
title: Windows 10 L2TP VPN 로그인 실패 대응
description: Windows에서 L2TP 프로토콜로 VPN 서버에 로그인할 수 없을 때 레지스트리를 수정하여 로그인한다.
category: tips
tags: [win10, VPN, L2TP]
comments: true
---

모든 윈도우10 컴퓨터에서 그런 건 아닌 것 같은데, 제가 손 본 열댓 대 컴퓨터 중 딱 한 대 빼고 동일한 증상이 있었습니다.
그건 _원격 서버가 응답하지 않기 때문에 컴퓨터에서 VPN 서버로 네트워크 연결을 할 수 없습니다_ 라는 메시지를 뱉는 에러입니다.
내부망이면 문제가 없는데 외부망에 서버가 있으면 발생하더군요.
이 문제를 해결하려면 레지스트리를 수정해야 합니다.

![레지스트리 편집기 실행](/postres/181223/win+r+regedit.jpg)

그냥 윈도우의 '시작'을 실행해서 regedit 이라고 입력해도 똑같이 나옵니다.
실행시 관리자 권한을 요구하므로 긴장하지 말고 확인을 누릅니다.

![키를 등록할 폴더로 이동](/postres/181223/regedit-policyagent.jpg)

레지스트리 편집기를 실행하면 다음 경로로 이동합니다.

```
컴퓨터\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\PolicyAgent
```

![DWARD32 키 추가](/postres/181223/regedit-new-dword32.jpg)

이제 키를 추가합니다.
키 종류는 _DWARD(32비트)값_ 입니다.

![키 이름 변경](/postres/181223/edit-item-name.jpg)

키를 추가하면 바로 아래에 _새 값 #1_ 이라고 생성됩니다.
이 상태에서 키 이름 **AssumeUDPEncapsulationContextOnSendRule** 을 입력합니다.

![키 값 변경을 위한 우클릭](/postres/181223/submit-new-key.jpg)

엔터를 누르거나 빈 화면을 클릭하면 저장됩니다.
입력한 키를 우클릭해서 보조메뉴를 연 후 _수정_ 을 클릭합니다.

![키 값 변경](/postres/181223/edit-key-value.jpg)

여기에서 데이터를 변경합니다. 데이터는 **2** 입니다.

![끝](/postres/181223/finish.jpg)

이제 재부팅하면 로그인할 수 있습니다.