---
layout: post
title: "[컴퓨터 네트워크] #8 App Layer - 2"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---



## 어플리케이션 레이어의 프로토콜

### 프로토콜이 정의하는 것들

실제 프로토콜에 대해 다루기 전에 어플리케이션 레이어의 프로토콜들은 어떤 것들을 정의해야 하는지에 대해 살펴보겠습니다.

<br/> 

우선 `request`와 `response` 같이 `메시지를 교환할 수 있는 형태`에 대한 정의가 필요합니다.

어떠한 필드를 사용할 것인지, 어떠한 방법으로 이 필드들을 **구분**할 것인지, 필드가 가질 수 있는 **길이**는 얼마인지 등의 메시지에 적용될 **문법**적 요소에 대한 정의가 필요하며 각 필드에 어떠한 **값**을 넣을지에 대한 합의도 필요합니다.

<br/> 

프로토콜의 가장 핵심인 `규칙`에 해당하는 내용도 정의되어야 하는데, 어떤식으로 구성된 메세지를 언제 보낼 것인지와 메시지를 받으면 어떤식으로 반응할 것인지 등에 대한 정의가 이에 속합니다.



### 프로토콜의 종류

`open protocol`은 IETF에서 제작되어 대중에게 공개되어있는 프로토콜로 `RFC`문서에 정의되어 있습니다.

HTTP, SMTP등의 프로토콜이 이에 해당합니다.

`proprietary protocol`은 회사 내부에서만 사용하기 위해 제작된 프로토콜로 Skype, Zoom 등이 이에 해당합니다.



## 앱 레이어가 받는 transport 서비스들

앱 레이어는 트랜스포트 레이어의 서비스를 제공받습니다.

트랜스포트의 서비스 중 어떤 서비스를 받을지 결정할 때 고려할 수 있는 요소들은 다음과 같습니다.

### Data Integrity (Reliability)

파일 다운로드와 같은 앱들은 무조건 reliablility를 제공해야 합니다.

파일을 전송하다 패킷이 유실되면 해당 파일은 쓸 수 없는 상태가 되기 때문입니다.

반면 스카이프 같은 앱들은 조금 로스가 나도 괜찮습니다.

음성이 데이터 패킷으로 전달될때 약간의 로스가 발생하더라도 이를 감내할 수 있기 때문이죠.



### Timing (Delay) & Throughput

게임, 영상통화와 같은 interactive 한 기능들은 딜레이가 적아야한다는 특징들을 가지고 있습니다. (사용자가 감당할 수 있는 쓰루풋의 한계가 정해져 있다는 말과도 같습니다.)

만약 영상통화를 하다 3초간의 딜레이를 거쳐 음성이 전달된다면 해당 앱은 상품성이 떨어지게 될 것입니다.

반면 딜레이가 조금 있더라도 1회성 작업이기에 큰 문제가 없는 파일 전송 같은 기능도 존재합니다.

> 쓰루풋의 한계가 없다는 말은 "쓰루풋이 elastic해도 된다."라고도 표현합니다.

### Security

암호화가 필요한 기능이라면 보안적 요소도 트랜스포트 레이어의 서비스를 결정하는데 있어서 중요한 요소가 될 수 있습니다.



## Transport services

### TCP

**Reliable**

Transmission Control Protocol(TCP)은 `reliable한 서비스`를 제공합니다.

reliability를 제공한다는 것은 no loss, ordering을 보장한다는 이야기와 같습니다.

패킷 로스가 발생했을때 이를 재전송을 통해 해결해주며, 모든 패킷이 순서대로 도착하지 않을 경우를 대비해 이를 순서대로 맞춰주는 것도 필요합니다.

> 라우터는 각각 독자적인 결정을 하여 포워딩하므로 모든 패킷이 전송되는 경로가 같지 않을 때도 있습니다.

위의 내용들을 구체적으로 어떻게 보장해주는지는 3장에서 다룰 예정입니다.

<br/> 

**Flow control**

TCP는 sender가 receiver에게 데이터를 전송할때 sender가 receiver를 `overwhelm`시키는 일을 막아줍니다.

buffer는 유한한 자원이기 때문에 receiver의 buffer 상태에 따라 sender가 데이터 전송을 미루어 receiver에서 데이터가 유실되는 경우를 방지합니다.

<br/> 

**Congestion control**

혼잡제어는 TCP를 사용하는 모든 sender에게 throttle을 걸어서 네트워크가 `overloaded` 되지 않도록 하는 방법입니다.

만약 쓰로틀링이 없이 네트워크가 모두 바쁘게 동작한다면 새로운 데이터 전송이 불가능 해지는 경우도 있기 때문이죠.

<br/> 

**Connection oriented**

TCP는 sender와 receiver간의 연결을 위해 환경변수 등의 내용들을 서로간에 주고받는 등의 작업이 필요합니다.

<br/> 

**Does not provide**

TCP는 timining, minimum throughput guarantee, security를 제공하지 못합니다.

<br/> 

### UDP

UDP는 User Datagram Protocol의 약자입니다.

L3 계층의 전송단위인 `datagram`이 이름에서 사용되는 이유는 L3 계층이 제공하는 서비스에 더해 UDP가 제공하는 서비스가 거의 없기 때문입니다.

UDP의 헤더도 L4 계층의 addressing을 위한 identifier(보내는쪽과 받는쪽의 port number) 외에는 중요한 내용이 거의 없습니다.

<br/> 

**Unreliable data transfer**

UDP는 L3 계층처럼 reliability를 제공하도록 노력은 하되, 보장은 하지 못합니다. (Best effort)

<br/> 

**does not provide**

UDP는 reliability, flow control, congestion을 제공하지 못하며

TCP에서와 마찬가지로 timining, throughput guarantee, security을 제공하지 못합니다.

<br/>

이렇게 TCP에서 제공하는걸 모두 제공하지 못하며 TCP에서 제공하던 기능들도 제공하지 못하는데도 UDP가 존재하는 이유는 UDP에서는 connection setup 작업이 필요하지 않기 때문입니다.

connection setup은 일종의 오버헤드에 속하므로 위의 기능들을 포기하더라도 오버헤드를 줄이는 선택을 할수도 있습니다.

> 서비스를 제공하지 못하지 못한다는 것은 반드시 저 작업들이 실패할 것임을 의미하는 것이 아님에 유의해야 합니다.
>
> Best effort를 통해 최선을 다해 서비스하지만 이것이 보장되지 않는 것 뿐이니 이를 오해해서는 안됩니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
