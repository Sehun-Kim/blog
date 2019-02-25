---
title: "[Network] 인증/권한"
layout: post
date: 2019-03-08
image:
headerImage: false
tag:
- http
- https
- Authorization
- Authenticate
category: blog
author: sehunkim
description: https의 탄생배경과 http와의 차잇점
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---

# 인증
인증을 위한 정보

- 패스워드 : 본인만 아는 문자열 정보
- 원타임 토큰 : 본인만이 가지고 있는 기기 등에 표시되는 한번 쓰고 버리는 패스워드 정보
- 전자 증명서 : 본인(단말기)만이 가지고 있는 정보
- 바이오 매트릭스 : 지문이나 홍채 등 본인의 신체 정보
- IC 카드

## HTTP/1.1의 인증방법

### 1. Basic 인증
웹서버와 대응하고 있는 클라이언트 사이에서 이뤄지는 인증방식

#### 인증 수순

```
1. 인증이 필요한 리소스에 리퀘스트 송신
2. 상태코드 401로 인증이 필요하다는 것 전달 (www-authenticate: basic realm="")
3. 유저ID와 PASSWORD를 Base64로 인코딩하여 송신
4. 인증 성공시 상태코드 200으로 응답 실패했을 경우 401로 응답
```

Base64라는 인코딩 형식은 암호화가 아니기 때문에 아무런 부가 정보 없이 복화화 가능함, 한번 Basic 인증을 하면 일반 브라우저에서는 로그아웃 할 수 없다.

### 2. DIGEST 인증

Basic 인증의 약점을 보완한 방식. Challenge Response 방식이 사용되고 있어, 패스워드를 있는 그대로 직접 보내는 일은 없다.

#### Challenge Response
![challenge Response](http://ocw.metu.edu.tr/pluginfile.php/2647/mod_resource/content/0/data%20protection%20II/chapterVII/002-4_1.jpg)

최초에 상대방에게 인증 요구를 보내고 상대에게서 받은 챌린지 코드를 사용해서 리스폰스 코드를 계산함. 이 값을 상대에게 송신하여 인증을 함

 Response Code라는 패스워드와 Challenge Code를 이용하여 계산한 결과를 상대에게 보내기 때문에 패스워드가 노출될 가능성이 줄어듬

#### 인증 수순

```
1. 인증이 필요한 리소스에 리퀘스트 송신
2. 서버가 401 Authorization Required 응답 하면서 인증이 필요하다고 전달.
   (www-authenticate: digest realm="", nonce"", algorithm="",qop="")
3. 클라이언트는 인증을 위해 필요한 정보를 반환한다.
   이 때 username, realm, nonce, uri, response는 반드시 포함 되어야 한다. realm과 nonce는 서버에서 받은 것을 사용 함
   (authorization: digest realm="", nonce="", username="", uri="", response="", algorithm="")
4. 서버는 성공시 200 실패시 다시 401 응답
```

> nonce는 챌린지코드이다. 401 리스폰스를 반환할 때 마다 생성되는 유일한 문자열이다.
>
> response는 리스폰스 코드이다.
>
> username은 realm에서 인증 가능한 사용자 이름



### 3. SSL 인증
만약 user id와 패스워드가 도난 당했을 경우 악의적인 사용자가 위장하여 접근할 수 있다.

SSL 방식은 https의 **클라이언트 인증서** 를 이용한 인증 방식으로 사전에 등록되어 있는 클라이언트 증명서가 있을 때 활용할 수 있다.


#### 인증 수순
사전에 클라이언트에 클라이언트 증명서를 배포하고 설치해 놓아야 한다.

```
1. 인증이 필요한 리소스에 리퀘스트를 보냄
2. 서버에서 증명서를 요구하는 "Certificate Request" 메시지를 응답함
3. user는 송신하는 클라이언트 증명서를 선택하고 서버에 "Client Cerificate" 메시지를 보냄
4. 서버는 클라이언트 증명서를 검증하여 검증결과가 정확하면 클라이언트의 공개키를 얻는다. 이 후 Https에 의한 암호를 개시함
```

> SSL 클라이언트 인증에는 증명서 발급을 위한 돈이 필요하다.

SSL 클라이언트 인증은 단독으로 사용되지 않고 폼베이스 인증과 합쳐서 2-factor 인증의 하나로서 이용된다.

즉,  패스워드라는 한 개의 요소만이 아니라 이용자가 가진 다른 정보를 합쳐 인증하는 방식

- SSL 클라이언트 인증을 하여 클라이언트 컴퓨터를 인증하고
- 패스워드를 사용해서 유저의 본인확인을 한다.



### 폼베이스 인증
http 프로토콜로 사양이 정의된 인증 방식은 아님. **클라이언트**가 **서버상의 웹 어플리케이션**에 자격 정보를 송신하여 그 자격 정보의 검증 결과에 따라 인증을 하는 방식

#### 인증수순
어플리케이션마다 다르게 만들 수 있다.

```
1. 임의의 user ID, PASSWORD를 폼에 입력하여 웹 어플리케이션에 송신한다.
2. 검증이 성공하면 Session에 인증 상태를 저장한다.
```
