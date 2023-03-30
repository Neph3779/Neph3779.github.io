---
layout: post
title: "[컴퓨터 네트워크] #12 App Layer - 6"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## DNS: Domain Name System

사람을 이름뿐만 아닌 주민번호, 여권번호 등 다양한 숫자로 구분하는 것처럼, 컴퓨터도 이름과 숫자로 구분이 가능합니다.

L3 계층은 출발지 컴퓨터로부터 목적지 컴퓨터로까지의 통신을 담당하므로 **목적지 컴퓨터의 주소지**가 필요합니다.

> 더 정확히는 목적지 컴퓨터의 네트워크 인터페이스까지의 통신을 담당합니다.

즉, IP라는 프로토콜은 목적지의 주소를 알아야 하며, 이 목적지의 주소를 나타내는 방법은 이름 (`hostname`)과 주소(`ip주소`)가 있습니다.

ip주소는 32bit의 주소체계이므로(IPV4 기준), 사람이 외우거나 작성하기 어려우니 사람은 이름(hostname)으로 통신하되, DNS서버를 통해 이를 ip주소로 바꿔서 전달합니다.

> Q. 그냥 이름을 가지고 통신할 수는 없나요? 왜 ip주소가 필요한가요?

만약 이름을 가지고 통신하게 된다면 IP 패킷의 헤더에 ip주소 대신 hostname이 들어가야 하는데, 고정적인 길이의 ip주소와 다르게 hostname의 길이는 가변적이기 때문에 라우터의 성능에 악영향을 끼치기 때문에 ip주소가 사용됩니다.

### DNS

DNS는 hostname을 ip주소로 바꿔주는 application layer의 프로토콜입니다.

DNS는 분산데이터베이스를 통해 서비스를 제공하며, 이런 분산데이터를 구성하는 서버는 `Name Server`라고 부릅니다.

> 한 컴퓨터에 ip주소 - hostname의 매핑 데이터가 전부 있는게 아니라 수없이 많은 DNS 서버가 이를 조금씩 나누어 보관하고 있습니다.

hostname은 본명에 해당하는 `canonical name`외에도 `host aliasing`을 통해 만들어진 별칭이 있을 수 있기 때문에 hostname을 ip주소로 바꿔주는 작업은 단순하지만은 않습니다.

또, DNS는 ip주소를 hostname으로 바꿔주는 서비스도 제공하는데, DDOS 공격이 가해졌을때 source ip주소를 가지고 공격을 행한 곳이 어디인지를 알기 위해 사용됩니다.

<br/> 

DNS 서비스가 제공하는 또 다른 주요 서비스는 `Load Distribution`(부하 분산)입니다.

만약 어떤 사이트가 매우 인기 있다면 하나의 서버로는 다수의 서비스 요구를 수용할 수 없으니 복제(replicate) 서버를 두고 운영합니다. 클라이언트 입장에서는 하나의 이름을 통해 접근하지만 DNS 서버는 복제서버들의 ip주소를 Round Robin 방식으로 번갈아가며 제공합니다.

<br/>

분산 데이터베이스인 DNS는 각각의 서버도 어마어마한 양의 데이터베이스를 보유하고 있습니다. 대부분의 DNS 서버는 매일 조단위 이상의 요청을 처리하는데, 이중 대부분은 `read 요청`에 해당합니다.

`write 요청`은 새롭게 호스트네임 - ip주소 매핑을 등록하는 경우에 발생하는데, ISP를 변경하는 경우에도 기존의 ISP가 사용중이던 ip주소 대역을 회수해가므로 새로운 등록이 필요하게 됩니다. 

> ISP를 바꾸면 반드시 ip주소가 바뀌는 것은 아닙니다. ISP끼리 계약을 맺어 이 작업을 하지 않을 수 있는데, 이는 뒤에서 다시 언급될 내용입니다.

<br/>

DNS서버는 신뢰성과 보안성이 중요합니다. 만약 DNS 서버의 매핑 내용이 잘못되어있다면 심각한 문제로 이어질 수 있으며 이에 대해서는 아래에서 더 자세히 설명합니다.

## DNS의 구조

![image-20230308144055265](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230308144055.png)

DNS는 분산 데이터베이스이자 계층적인 구조를 지닌 데이터베이스입니다.

가장 상단의 Root 서버를 시작으로 tree 형식으로 구성된 계층도를 볼 수 있습니다.



### Root DNS Servers

Root 서버는 클라이언트의 `요청에 대한 답을 알고 있을법한 사람을 알고있는` 마지막 서버입니다.

클라이언트가 어떤 hostname의 ip주소를 local DNS 서버에 물어보면 local DNS 서버는 이에 대한 답을 알고 있을법한 DNS서버에 다시 질문을 하게 됩니다. (local DNS서버가 요청에 대한 답을 모르는 경우)

이 과정이 반복되어 마지막으로 질문하러 올라가는 곳이 Root DNS 서버입니다.

만약 Root DNS 서버에서조차 "어떤 DNS 서버가 알고 있을지"에 대한 답을 주지 못한다면 더이상 name resolution은 진행되지 않습니다.

> name resolution:  hostname을 ip주소로 변환하기 위해 질문을 주고받는 일



### Top-Level Domain

Top Level Domain은 쉽게 말해 .com, .net, .org등을 담당하는 계층입니다.

.kr, .jp, .uk 등의 국가 식별자인 CCTL(Country Code Top Level)도 TLD에 속합니다.

.com으로 끝나는 hostname이라면 .com DNS 서버에 질문을 보낼 수 있고,

만약 TLD가 ip주소를 알고있다면 ip주소를 반환하고, 아니라면 child DNS 서버에 다시 질문이 가게 됩니다.



### Authoritative DNS servers

Authoritative DNS 서버는 권위있는 DNS 서버라고 번역할 수 있는데, 여기서 권위가 있다는 의미는 정확한 답을 줄 수 있다는 의미와도 같습니다.

어떤 기관에서 자체적으로 운영하는 DNS라면 그 기관 소속에 대한 질문은 정확한 답(권위있는 답)을 줄 수 있으므로 기관 DNS 서버는 authoritative DNS 서버가 됩니다.

기관의 DNS 서버는 꼭 자체적으로 운영되지 않을 수도 있으며, ISP의 도움을 받을수도 있습니다. (ISP가 name server 서비스를 제공하는 경우)



### Local DNS name server

Local DNS 서버는 클라이언트가 가장 먼저 접근하는 DNS server입니다.

이미 캐싱되어있는 정보에 대해 물어봤다면 바로 답해주며, 답을 모른다면 DNS name resolution을 위한 쿼리를 전달합니다.



### DNS 서버의 보안

DNS서버의 보안은 `DNSSEC`이라는 프로토콜을 통해 관리됩니다.

DNS 서버의 내용을 바꿔치면 아래의 시나리오처럼 심각한 문제가 발생할 수 있습니다.

1. 클라이언트가 웹브라우저의 주소창에 hostname을 입력하여 웹페이지로 이동합니다.
2. DNS server의 name resolution이 끝나 전달받은 ip주소로 이동합니다.
3. 사용자는 이동된 웹페이지를 전적으로 신뢰합니다.

너무나 일반적인 웹페이지 이동 시나리오이지만, 만약 DNS server의 데이터베이스가 오염되어 해커가 바꿔치기한 ip주소를 전달받았다면, 게다가 바꿔치기한 ip주소를 통해 이동한 웹페이지가 정말 이동하려던 웹페이지를 그대로 베껴와서 모양이 같다면, 사용자는 아무런 의심없이 웹페이지에서 작업을 하게 됩니다.

만약 이 웹페이지에서 개인정보를 요구하더라도, 의심없이 개인정보를 입력하게 될 수 있는 것이죠.

이러한 문제 때문에 DNS 서버의 내용을 변경하기 위해서는 인증(authorization) 과정이 필요합니다.

또한 name resolution을 요청한 message의 integrity 훼손여부를 입증하여 중간에 변경되지 않은 요청임을 증명해야합니다.



## DNS name resolution

hostname을 ip주소로 바꿔주는 name resolution에는 아래와 같이 두 종류의 방법이 존재합니다.

### Iterated query 방식

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230308144647.png" alt="image-20230308144647004" style="zoom: 33%;" />

클라이언트는 name resolution을 하기 위해 local DNS 서버에게 질문을 보냅니다. 

이때 local DNS 서버의 주소는 처음 컴퓨터를 네트워크에 연결시키며 ip주소를 세팅할때 local DNS 서버 ip주소를 같이 세팅해놓으므로 local DNS 서버의 주소를 찾아가는 과정은 필요하지 않습니다.

그럼 loacl DNS는 정답을 알만한 서버에게 질문을 하게 되고, 해당 서버는 또 다시 정답을 알만한 서버를 local DNS 서버에 전달합니다.

이처럼 local DNS 서버가 "정답을 알만한" DNS 서버의 주소를 전달받고, 해당 주소에 질문을 보내는 일을 반복하다 최종적으로 정답을 아는 서버로부터 정답을 받아오는 방식이 `iterated query` 방식입니다.

`Iterated query` 방식은 가장 일반적으로 사용되는 name resolution의 방식입니다.

> 설명을 위해 사용된 그림의 예시와는 다르게 일반적으로는 local DNS가 TLD를 캐싱해서 가지고 있기 때문에 root DNS 서버까지 질문이 올라가지는 않습니다.

### Recursive query 방식

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230308144718.png" alt="image-20230308144718822" style="zoom: 33%;" />

위에서 설명한 iterated query 방식과는 다르게 recursive 방식을 local DNS server가 정답을 알만한 서버에 질문을 보낸뒤 질문을 받은 서버가 다시 정답을 알만한 서버에게 질문을 보내는 재귀적인 방식을 통해 최종적인 답을 전달받는 방식입니다.

<br/>

이 방식은 다른 DNS 서버에 부담을 지우는 방식이므로 잘 사용되지 않습니다. 

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
