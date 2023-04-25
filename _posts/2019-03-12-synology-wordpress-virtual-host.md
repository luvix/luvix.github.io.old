---
layout: post
title: Synolog NAS에 설치한 워드프레스 주소를 가상 호스트 주소로 바꾸기
description: 시놀로지 나스에 설치한 워드프레스 주소를 하위 디렉토리가 아닌 가상 호스트 주소로 바꿔본다.
category: tips
tags: [synology, wordpress, virtualhost]
comments: true
image: https://user-images.githubusercontent.com/16158188/54201098-31def780-4510-11e9-965b-67b8f5dead86.jpg
---

## wp-config.php 수정

써야 되는 지는 모르겠지만 일단 썼습니다.
FTP난 SSH로 `../web/wordpress/wp-config.php` 파일에 다음 한 줄을 추가합니다.

``` php
define('RELOCATE',true);
```

## 데이터베이스 수정

![vpnprofile](/postres/181223/press.scrawl.org0.jpg)

만약 home과 siteurl가 뭐가 다른 지 궁금하다면 [여기](https://www.thewordcracker.com/basic/how-to-solve-problems-after-changing-the-site-url-in-wordpress/)에서 확인하세요.
그리고 접속하면 됩니다.

## 가상호스트 설정

설정을 끝내면 시놀로지 웹으로 접속햇 **Web Station**을 실행합니다. 그리고 **가상호스트**를 선택해 메뉴로 들어갑니다. 그리고 추가를 눌러 다음과 같이 입력합니다.
이 때 `HTTP 백엔드 서버`는 반드시 Apache 2.2로 합니다. Nginx 는 안 되더군요. php 는 적당히 골라주세요.

![vpnlogin](/postres/190312/press.scrawl.org1.jpg)

이상입니다.

## Reference

- [codeX: 사이트 URL 변경 방법(KR)](https://codex.wordpress.org/ko:%EC%82%AC%EC%9D%B4%ED%8A%B8_URL_%EB%B3%80%EA%B2%BD_%EB%B0%A9%EB%B2%95)
- [워드프레스 주소 변경으로 사이트에 접속하지 못하는 문제 해결](https://www.thewordcracker.com/basic/how-to-solve-problems-after-changing-the-site-url-in-wordpress/)