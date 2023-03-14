---
layout: post
title: "[컴퓨터 네트워크] #14 App Layer - 8"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## DASH

DASH는 비디오 스트리밍 제공을 위한 프로토콜입니다.

DASH는 클라이언트가 적절한 CDN Node를 골라 chunk를 받아갈 수 있다고 가정합니다. (intelligence client를 가정)

클라이언트는 `언제(when)` `무엇(what)`을 `어디서(where)` 가져갈 것인지 결정하게 됩니다.

`when`: chunk를 요청하는 시점 (buffer가 어느정도 비워졌을때 요청할 것인지)

`what` : 어떠한 화질로 요청할 것인지

`where`: 어느곳에 위치한 chunk를 요청할 것인지 (대역폭을 가지고 판단)

즉 Streaming video는 encoding 기법 + DASH + buffering의 합작이라 볼 수 있습니다.



## CDN

CDN을 운영하게 되면 수많은 사용자가 서비스를 요청할때 어떻게 처리할 것인지가 중요합니다.

모든 비디오를 모든 CDN 서버에 똑같이 저장해놓고 있는 것이 가장 이상적이지만 이는 매우 비효율적입니다.

어느 지역에서 어떤 서비스를 요청할지에 대해 예측해 해당 지역의 CDN에 저장해놓아야 효율적인 운영이 가능합니다.

CDN 서버는 다수가 존재하는데, 각각의 서버에 `chunk`와 `manifest file`이 존재하게 됩니다.

`manifest file`은 chunk들의 설명서로, 어떤 CDN 서버에 어떠한 chunk가 있는지에 대한 정보 또는 chunk들의 구성, 각각의 chunk들의 화질 등에 대한 정보가 담겨있습니다.

CDN 서버를 운영하는 방식은 크게 `Enter deep` 방식과 `Bring home` 방식이 있습니다.

### Enter deep

Enter deep은 CDN 서버를 최대한 유저와 가까이 두자는 전략입니다.

유저가 가장 처음으로 네트워크에 진입하는 곳은 액세스 네트워크입니다.

Enter deep 기법에서는 액세스 네트워크의 가까이에 CDN 서버를 두고 운영합니다.

response time이 짧아지는 장점이 있지만 많은 서버를 운영해야 한다는 단점이 존재합니다.

### Bring home

Bring home 기법은 IXP에 강력한 서버를 소수만 배치하는 전략입니다. (Enter deep에 비해 상대적으로 소수)

IXP는 ISP들의 연결 지점인데 이곳 주변에 성능이 좋은 서버를 배치하여 서비스를 운영합니다.



### Case study: Netfilx

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230314165720.png" alt="image-20230314165720511" style="zoom: 25%;" />

넷플릭스는 CDN 서비스를 운영하는 회사에 서비스를 맡기지 않고 자체적으로 CDN 서비스를 구축하여 운영하고 있습니다.

클라이언트는 아마존 클라우드를 통해 어떤 CDN 서버를 선택해야 하는지를 응답받고, 해당 CDN 서버로 가서 서비스를 받게 되는 구조로 운영되고 있습니다.

이때 아마존 클라우드는 CDN 서버의 위치만을 저장하며, chunk나 manifest file은 저장해두지 않습니다.



## Socket Programming

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230314171504.png" alt="image-20230314171504351" style="zoom: 33%;" />

Socket은 Application Layer와  Transport Layer 사이에 존재하는 인터넷 응용 프로그램을 만들기 위한 API입니다.

인터넷 어플 중 클라이언트 어플리케이션을 만들때 사용하며, 어플리케이션 레이어 하위의 일을 모두 추상화해줍니다.

>  Transport layer부터 Physical layer는 OS의 kernel에 구현되어있는 영역입니다.



소켓 프로그래밍은 UDP와 TCP를 모두 사용할 수 있습니다. (SCTP 같은 프로토콜들도 사용 가능합니다.)



### UDP Socket Interaction

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230314172407.png" alt="image-20230314172407415" style="zoom: 25%;" />

UDP는 클라이언트와 서버 사이에 핸드쉐이킹(connection) 없습니다.

데이터를 보내기 전에 사전작업이 필요 없으며, 서버의 주소(ip주소와 포트번호)를 미리 알고 있어야 합니다.

서버는 서비스를 제공하기 위해 특정 포트번호의 포트를 열어놓고 기다리게 되며

데이터를 수신하게 되면 서버(recevier)는 클라이언트(sender)의 ip주소를 보고 기다리고 있던 데이터가 맞다면 해당 port 번호로 데이터를 수신합니다.



### TCP Socket Interaction

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230314173218.png" alt="image-20230314173218343" style="zoom: 25%;" />

TCP는 연결 설정이 필요합니다. (TCP connection)

클라이언트가 서버에 접근하면 서버는 클라이언트의 요청을 수락하거나 거부합니다.

만약 수락한다면 클라이언트를 위해 별도의 소켓을 제작해주게 되고 (소켓에는 IP 주소, 포트번호가 저장되어 있음)

클라이언트는 만들어준 소켓을 가지고 해당 주소로 연결을 요청합니다.

처음 요청받은 포트번호는 다른 클라이언트의 요청도 수신해야 하므로 소켓을 제작할때는 처음 요청받은 포트번호가 아닌 다른 포트번호를 담아 제작하게 됩니다.

클라이언트는 포트번호를 가지고 구분할 수 있습니다.

