---
layout: post
title: "[컴퓨터 네트워크] #6 Introduction - 6"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Protocol Layer

### OSI 7 Layer

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230215211149.png" alt="image-20230215211149767" style="zoom:33%;" />

ISO가 규정한 OSI 7 Layer는 서로 다른 네트워크들을 연결하기 위해 제시된 설계 모델입니다.

현재는 저 7계층을 엄격하게 지키는 경우는 거의 없으며, 대부분 5계층만 사용되고 있습니다. 

OSI 7 Layer는 말 그대로 참조의 역할(레퍼런스)로만 사용되고 있습니다.

> OSI는 Open System Interconnection의 약자입니다.



### Protocol Layer 설계

복잡한 시스템을 설계할때 흔히 쓰는 방법은 복잡한 내용을 다룰만한 수준까지 분해하여 그 각각을 설계하고, 그것들을 모두 구현한 후에 다같이 합쳐 큰 문제를 해결하는 기법인 `분할정복 기법`입니다.

이 아이디어를 가지고 설계한 모델이 성공하여 지금의 인터넷 계층 구조를 이루게 되었습니다.



### 인터넷 계층 구조 

인터넷 계층 구조는 레이어 `N`이 `N-1`로부터 서비스를 받아 `N+1`에 서비스를 제공하도록 설계되어 있습니다.

레이어 `N`은 `N+1`에 제공하는 서비스의 **인터페이스에만 맞게 제작한다면** 어떤 방식으로 서비스를 제공하든 상관이 없으며, 이러한 설계 방법은 **유지보수** 측면에서 큰 장점을 가져옵니다.

이 원리에 대해 이해한다면 각 레이어가 제공하는 서비스를 기계적으로 암기하는 것이 아닌, 왜 그런 서비스를 제공할 수 밖에 없는지에 대해 이해할 수 있게됩니다.



## 5계층 레이어 구조

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230217210806.png" alt="image-20230217210806846" style="zoom:50%;" />

### L5 - application

L5 계층은 `어플리케이션 레이어`로, 웹 브라우저와 같은 **어플리케이션을 구동시키기 위한 인터페이스**를 제공합니다.

어플리케이션 레이어에서 사용되는 대표적인 프로토콜로는 `HTTP`(웹), `IMAP`(이메일 전송), `SMTP`(이메일 수신), `DNS`(도메인 이름을 ip로 바꾸어주는 서비스) 등이 있습니다.

어플리케이션 레이어에서 보내는 전송 단위는 `message`라고 부르며 규정된 인터페이스에 맞추어 제작되어 transport 계층에 이를 전달합니다.

 

### L4 - transport

L4 계층에서는 `transport 레이어`로, 어플리케이션 레이어에 서비스를 제공하는 계층입니다.

어플리케이션 레이어가 동작하기 위해 필요한 서비스는 **데이터의 송수신**입니다.

데이터를 주고받는 당사자는 process들이므로 transport 레이어는 **process - process 통신**을 제공합니다.

transport 레이어에서 사용되는 프로토콜로는 신뢰성을 보장하는 대신 느린 `TCP`와 그렇지 않은 대신 빠른 `UDP`가 있습니다.

transport 레이어는 어플리케이션으로부터 받은 메시지에 데이터에 대한 제어정보를 담은 header를 부착하여 L3계층에 이를 전달합니다.

transport 레이어의 전송 단위는 `segment`라 부르며, 어플리케이션으로부터 받은 메시지의 단위가 크다면 이를 적절히 잘라 (인터페이스에 맞게) 각각에 헤더를 부착하여 L3 계층으로 내려보낼 수도 있습니다.

이렇게 적절히 데이터를 잘라 헤더를 붙이는 행위를 `encapsulation`이라 합니다.

> transport 레이어의 전송단위는 TCP의 경우 segment, UDP의 경우 datagram이라 부릅니다.
>
> datagram은 L3 계층의 용어가 아닌가 의문이 드시겠지만 이는 추후에 다시 설명할 내용입니다.

 

### L3 - network

L3 계층은 `네트워크 레이어`로, L4 계층의 서비스인 process - process 통신이 가능하도록 하기 위해 **computer - computer 통신**을 지원합니다.

즉, **routing 서비스**를 제공하는 것이죠. 더 정확히 말하자면 시작지의 네트워크 인터페이스와 목적지의 네트워크 인터페이스 사이의 데이터 전달을 맡고 있는 것입니다.

네트워크 계층에서 사용하는 전송 단위는 `datagram` 또는 `packet`이라고 부르며, `IP`라는 프로토콜을 사용합니다.

네트워크 계층에서도 마찬가지로 데이터 앞에 헤더를 부착하는데 transport 레이어에서 내려준 전체가 데이터에 해당합니다.

즉, **transport 레이어가 부착한 헤더**도 네트워크 계층의 입장에선 **데이터**에 해당합니다.

 

### L2 - link

Internet은 network들의 network입니다.

`링크 레이어`라 불리는 L2 계층은, L3 계층이 제공하는 컴퓨터와 컴퓨터 사이의 통신을 제공하기 위해, **네트워크와 네트워크 간의 통신**을 지원해야 합니다.

네트워크들을 거쳐 지나가 최종 목적지 네트워크에 도착할수 있도록 네트워크 - 네트워크 간의 이동을 책임져주는 것이 L2 계층의 역할입니다.

가장 널리 사용되는 L2계층의 기술이 `이더넷`이기 때문에 L2 계층의 전송 단위도 이더넷 프레임에서 따온 `frame`이란 용어를 사용하며, 프레임은 L3 계층으로부터 전달받은 데이터에 `링크 헤더`를 부착하는 형식으로 제작됩니다. 



### L1 - physical

`피지컬 레이어`라 불리는 L1 계층은 **네트워크 내부의 데이터 전송**을 담당합니다.

L1 계층은 L2 계층에 실제 물리적인 경로를 (유선 / 무선) 제공하여 네트워크 내부에서의 이동이 이루어지도록 해줍니다.



### Encapsulate / Decapsulate

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230217230755.png" alt="image-20230217230755307" style="zoom: 33%;" />

송신자로부터 각 계층을 내려오며 헤더필드를 추가하는 작업을 Encapsulate

수신자로부터 각 계층을 올라가며 헤더필드를 검사하며 떼어내는 작업을 Decapsulate라고 합니다.

<br/>

어플리케이션에서 보낸 메세지는 encapsulate 되어 physical 링크를 타고 switch에 도달합니다.

도착한 데이터는 스위치와 라우터의 지정된 계층까지 decapsulate되었다가 다시 encapsulate되며

최종적으로 도착하는 목적지에서 app layer까지 decapsulate되어 message를 전달받습니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
