---
layout: post
title: "[컴퓨터 네트워크] #9 App Layer - 3"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Securing TCP

TCP는 자체로 보안기능을 제공하지 못합니다.

암호화를 지원하지 않으니 입력한 비밀번호 텍스트가 그대로 전달되게 되는데

이를 막기 위해 있는게 `TLS` 프로토콜입니다. 

> TLS는 SSL에서 이어져 나온 표준 프로토콜입니다.

TCP connection에서 주고받는 정보를 암호화하며 데이터 훼손 여부를 검사하고 각각의 End point가 올바른 사용자가 맞는지를 인증하는 체계인 `End-point authentication`도 가지고 있습니다.

<br/>

TLS는 어플리케이션이 사용할 수 있는 API의 형태로 제공되며

소켓에 비밀번호 등의 텍스트가 전달되면 이를 암호화시켜 목적지로 전송합니다.



## Web and HTTP

웹은 90년대 초반에 등장한 인터넷 어플리케이션입니다.

웹 서버에서 제공하는 object를 받아 브라우저에 display하는 역할을 하는 웹은

초창기에는 대부분 사용자 customize된 페이지가 아닌 고정된 내용이 누구에게나 보여지는 형태였습니다.

즉, static한 내용만을 보여줬던 것이죠.

<br/>

Dynamic web을 위한 기술들이 등장함에 따라 사용자 특화 페이지가 생겨났고

자바 스크립트의 등장으로 브라우저에서 코드를 돌릴 수도 있게 되었습니다.

하지만 이 글에서는 http connection에 대해서만 설명할 것이므로 웹페이지는 모두 static한 web page라고 가정하겠습니다.

<br/>

object들로 이루어진 웹 페이지들은 다양한 object가 홈페이지 하나에 존재하는데

이 object들이 각기 다른 웹 서버에 저장되어있을 수 있습니다.

object의 종류로는 html file, jpeg image, java applet, audio file 등이 존재하며

하나의 웹페이지에 다양한 오브젝트가 참조되어있는 형태를 띕니다.

이러한 웹 페이지는 URL을 통해 주소를 특정하여 사용합니다.



### HTTP 

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230221183707.png" alt="image-20230221183707536" style="zoom:33%;" />

HTTP는 hypertext transfer protocol의 약자로 웹 어플리케이션 레이어 프로토콜입니다.

> hyper text는 web object입니다.



### HTTP 레이어 구조

| Web  |
| :--: |
| HTTP |
| TCP  |

Web, HTTP, TCP는 위와 같은 레이어 구조를 띄게 되므로 HTTP는 TCP가 제공하는 서비스를 이용하게 됩니다.

HTTP는 Client - Server 모델(패러다임)을 따르므로 Client는 브라우저를 통해 웹 오브젝트를 요청하고 이를 display하는 역할을, Server는 HTTP Request 받아 Client에게 HTTP Response를 돌려주는 역할을 하게됩니다.

<br/> 

이러한 작업들은 `TCP connection` 위에서 진행되는데 `TCP connection`이 이루어졌다는 것은 데이터를 주고받을 두 process가 각각 buffer 공간, 변수, 자원할당이 이루어졌다는 의미이기도 합니다.

TCP 연결 여부는 process ~ TCP레이어에서만 알고 있으며, 그 하위 영역에서는 연결된 상태에 대해 알지 못하므로 logical한 연결상태입니다.

<br/> 

Client가 server로 TCP connection 요청을 하면 (일반적으로 웹이 사용하는 포트 번호인 80을 사용)

서버가 이를 받아들일지 선택한 후, 받아들인다면 이 연결을 통해 HTTP 메시지(Request, Response)를 주고받게 됩니다.

<br/>



HTTP는 `stateless protocol`입니다.

`state`란 상대방에 대한 정보로 클라이언트 입장에서 설명하자면 현재 서버가 보내주는 오브젝트들에 대한 정보가 이에 속합니다.

`stateless`라는 것은 클라이언트도, 서버도 서로의 상태를 실시간으로 알고 있지 못하다는 이야기이죠.

클라이언트의 상태를 기록해두게 되면 웹 서버의 부담이 커지게 될 것이며, 서버나 클라이언트의 컴퓨터가 고장나거나 에러가 발생하여 state를 잃어버린 경우 기존의 state를 다시 복구하기 어렵기 때문에 `stateful`로 설계하지 않게 되었습니다.



## HTTP connection의 종류

### Non-persistent HTTP

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230221185713.png" alt="image-20230221185713046" style="zoom:33%;" />

`Non-persistent HTTP`는

하나의 오브젝트를 보내고 TCP 연결을 닫는 형태입니다.

오브젝트가 n개라면면 TCP 연결을 열었다 닫았다를 n번 해야하기 때문에 오버헤드가 큽니다.

3-way handshake라 부르는 과정을 통해 데이터를 주고받게 되는데

3-way handshake란 악수(데이터를 받을 준비)를 위해 3개의 메시지(TCP 연결 요청, 승인 메시지 수신, HTTP Request 전송)가 필요하다는 의미를 지닌 용어입니다.



### Persistent HTTP

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230221190840.png" alt="image-20230221190840489" style="zoom:33%;" />

`RTT`는 Round Trip Time의 약자로 패킷이 돌아오기까지의 시간의 측정값을 의미합니다.

이때 transmission delay의 영향을 최소화하여 propagation delay에 큰 비중을 두고 측정하기 위해 패킷의 크기를 아주 작게 해서 보내게 됩니다.

TCP 커넥션 연결이 승인되면 서버는 이 연결을 클라이언트가 요청한 다수의 오브젝트가 전달될때까지 유지하다가 전달이 끝나면 연결을 닫습니다.

이때 최종적으로 데이터를 받기 위해 필요한 시간은 `2RTT + transmission delay`가 됩니다.

