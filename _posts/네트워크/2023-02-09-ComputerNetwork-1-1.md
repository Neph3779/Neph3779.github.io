---
layout: post
title: "[컴퓨터 네트워크] #1 Introduction - 1"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## 5 Layered Architecture

- Application Layer
- Transport Layer
- Network Layer
  - Data Plane: Ip Protocol
  - Control Plane: Routing
- Data Link Layer
- Physical Layer

네트워크는 총 5가지의 레이어로 나뉩니다.

컴퓨터 네트워크 과목에서는 상위 3개의 레이어 (App, Trans, Net)를 다루게 됩니다.



## 1장에서 다루게 될 내용들

- 인터넷이란 무엇인가?
  - 프로토콜이란 무엇인가?
- 하드웨어적 관점에서 Network edge와 Network core의 차이
  - Network edge: hosts, access network, physical media
  - Network core: packet/circuit switching, internet structure
- 성능
  - loss(유실), delay(지연), throughput
- Protocol Layer, Service models
  - Service N(S_n)이 S_n-1로부터 어떤 서비스를 제공받아 S_n+1에 제공하는가
- 네트워크 보안
- 네트워크의 역사



## 인터넷의 구성요소

### Internet과 internet의 차이

> Q. Internet(1)과 internet(2)는 같나요?

정답은 서로 다르다입니다.

일상적으로 "인터넷이 안돼", "인터넷에 접속했어"와 같이 사용하는 인터넷은 1번에 해당하는 *고유명사*입니다.

2번은 *일반적인 용어* 에 해당합니다. 즉, network of networks라는 뜻을 지닌 단어입니다.

따라서 1번은 2번에 포함되는 관계입니다.

<br/>

**[예시]**

본사와 지사의 네트워크를 이어주는 네트워크 = internet

> Q. Internet에 대해 배우는 이유가 뭔가요?

*컴퓨터 네트워크* 중 가장 유명한 것이 바로 Internet입니다.

컴퓨터 네트워크 과목은 Internet에 어떤 기술이 사용되고 있는지를 배우는 과목입니다.



### 하드웨어 구성요소들

- **Device**
  - `Host` (End System) 라고도 불립니다.
  - 어플리케이션이 동작할 수 있도록 구동시켜주는 역할을 합니다.
  - 인터넷 상에서 host의 위치를 `Edge`라고 합니다. (네트워크에 비해 상대적 외부에 있다는 의미)
- **Packet switches**
  - 라우터와 스위치로 구성되며, 이들은 패킷을 교환하는 일을 합니다.
  - 라우터는 엄밀히 말하면 L3 스위치라고 볼 수 있습니다.
  - 문맥에 따라 스위치를 L2 스위치를 부르는 단독 용어로 사용하는 경우가 있으니 주의합시다.
    - L2 스위치의 예시: `Ethernet 스위치`
    - `Ethernet`은 데이터 통신 과목에서 배우는 내용중 가장 중요한 내용입니다. (가장 널리 사용되는 L2 기술이기 때문)
  - 패킷: 메세지를 적절히 나누어 보내는 상황에서, 이를 나누는 단위
  - 스위치와 라우터는 패킷이 도착하면, 도착한 패킷을 다음 목적지로 보내게 됩니다.
    - 이 작업을 `forwarding`이라고 합니다. (`routing`과 다른 개념으로, 목적지를 결정하는 단계가 아닌, 정해진 목적지로 패킷을 신속하게 전달하는 단계에 해당합니다.)



호스트, 스위치 모두 일종의 장치(컴퓨터)에 해당합니다.  (다만 굉장히 특수한 일을 하는 컴퓨터라고 볼 수 있는 것이죠.)

컴퓨터끼리 데이터를 주고받기 위해서는 (호스트, 스위치 간의 데이터 전송) 이를 연결해줄 어떠한 매체가 필요합니다. 

그 연결 매체가 바로 `Communication Link`입니다.

- **Communication Links**
  - Physical Layer(L1)에 해당하는 영역
  - 광섬유, 구리선, 라디오 전파, 위성 등을 통해 두 디바이스 사이를 연결하는 계층
  - Bandwidth(대역폭): 전송 효율을 결정하는 가장 중요한 요소



앞서 설명한 호스트, 패킷 스위치, 커뮤니케이션 링크를 합해 *네트워크* 를 제작할 수 있습니다.

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230209180311.png" alt="image-20230209180305512" style="zoom: 33%;" /> |
| :----------------------------------------------------------: |
|                  네트워크의 하드웨어적 구성                  |

### 소프트웨어 구성요소들

- **Internet**
  - 가장 대표적인 "네트워크들의 네트워크"입니다.
  - 다수의 ISP들의 연결로 구성되어있습니다.
    - ISP란 Internet Service Provider의 약자로, 우리나라로 치면 Kt, Skt, LG U+ 정도를 생각하면 됩니다.
    - ISP의 네트워크에 연결된 상태를 "Internet에 접속한 상태"라고 볼 수 있습니다.
    - ISP간의 연결도 존재합니다.
- **Protocol**
  - Internet은 TCP/IP 프로토콜에 의해 동작합니다.
- **Internet standards**
  - IETF라는 국제 단체에서 인터넷 표준(RFC)을 제작합니다.

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230209190111.png" alt="image-20230209190111682" style="zoom:33%;" /> |
| :----------------------------------------------------------: |
|                 네트워크의 소프트웨어적 구성                 |

### 서비스 관점에서의 인터넷

인터넷 자체로는 단순히 하드웨어가 연결된 것이 다이므로 실질적인 서비스를 제공하지는 못합니다.

서비스를 제공하는 것은 인터넷을 사용하는 어플리케이션이며 인터넷은 *서비스 제공을 위한 기반* 에 해당하는 역할을 합니다.

인터넷은 어플리케이션을 만들 수 있는 API역할을 하기도 하는데

신뢰성을 제공해야 하는 어플리케이션이 TCP를 이용하는 것이 API로써의 인터넷을 사용하는 예시입니다.

도착한 패킷을 신속하게 다음 목적지에 전달해주는 작업도 인터넷이라는 API가 수행해주는 동작이라고도 볼 수 있겠습니다.

이런 추상화된 API(Internet) 덕분에 어플리케이션을 만드는 사람은 데이터를 보내고 받는 것에만 집중할 수 있습니다.



### 프로토콜이란?

프로토콜은 메세지의 형태, 순서와 메세지 송수신에 대한 액션에 대한 정의(규칙, 약속)입니다.

어떤 형태로 메세지를 주고받을지, 발생한 이벤트를 어떻게 처리할 것인지에 대한 모든 약속이라 말할 수 있겠습니다.



### Network Edge와 Access Network

네트워크 edge는 호스트를 나타내며, 호스트는 서비스를 제공받는 client와 서비스를 제공하는 server로 구분됩니다.

액세스 네트워크는 호스트를 인터넷에 연결시키기 위해 필요한 네트워크입니다.

순서의 흐름으로 본다면 `호스트 -> 액세스 네트워크 -> 네트워크 코어 -> 액세스 네트워크 -> 호스트`의 흐름으로 데이터가 전달됩니다.

그림으로 본다면 아래의 영역이 액세스 네트워크에 해당합니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230209192114.png" alt="image-20230209192114423" style="zoom:33%;" />

액세스 네트워크는 유선/무선일 수 있습니다.

만약 스마트폰으로 인터넷에 접속하고 있다면 스마트폰과 Wifi의  AP, 혹은 4G나 5G의 Base Station까지의 연결이 액세스 네트워크에 해당합니다.



### Network Core

네트워크 코어에는 스위치, 라우터 중 라우터만 포함되어 있습니다.

코어에는 호스트가 직접 연결되어있지 않으며, 액세스 네트워크에서 발생한 트래픽을 전달해주는 역할만을 합니다.

`네트워크들이 이루고 있는 네트워크`를 네트워크 코어라 부를 수 있습니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
