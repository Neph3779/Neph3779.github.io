---
layout: post
title: "[컴퓨터 네트워크] #4 Introduction - 4"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## 인터넷 구조 - 네트워크들의 네트워크

인터넷은 `네트워크들의 네트워크`입니다.

네트워크들은 어떤식으로 모여져서 하나의 네트워크가 되는걸까요? 

<br/> 

먼저 호스트는 Access ISP를 통해 인터넷에 연결됩니다.

KT를 사용하는 유저라면 KT(ISP)의 Access network를 통해 연결되는 것이죠.

"**인터넷에 연결되어있다"**는 말의 의미는 인터넷에 연결된 그 어떠한 장치와도 패킷을 주고 받을 수 있다는 의미이므로 이론적으로 어떠한 위치에 있는 두 호스트도 반드시 연결이 가능해야 합니다.

KT를 사용하는 유저와 SKT를 사용하는 유저를 연결하기 위해선 ISP들도 서로 연결되어있어야 합니다.

결론적으로 모든 ISP들이 연결되어 있어야 하기에 인터넷의 구조는 상당히 복잡할 수 밖에 없습니다. 

### 액세스 네트워크를 연결하는 방법들

액세스 네트워크들을 서로 연결하는 방법에는 어떤 것들이 있을까요?

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213182707.png" alt="image-20230213182707397" style="zoom:33%;" />

가장 간단한 방법으로는 모든 액세스 네트워크끼리 서로 직접 연결되어있도록 하는 방법이 있습니다.

이 방법은 당연하게도 채택될 수 없습니다. 현존하는 모든 ISP를 연결하는 복잡도도 너무 클 뿐더러, 만약 새로운 액세스 네트워크가 생기면 현존하는 모든 액세스 네트워크에 연결해줘야 하기 때문이죠.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213182724.png" alt="image-20230213182724522" style="zoom:33%;" />

또 다른 방법으로는 하나의 글로벌 ISP를 두어, 모든 액세스 네트워크가 글로벌 ISP를 거쳐가도록 하는 방법이 있습니다.

하지만 이 방법은 글로벌 ISP가 시장을 독점하게 되는 방법이므로 채택될 수 없습니다.



### 채택되고 있는 방법

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213184341.png" alt="image-20230213184341920" style="zoom: 50%;" />

결론적으로 현재 채택되어 운영되고 있는 인터넷 구조는 `IXP`가 `ISP`들을 연결하는 구조입니다. ISP들끼리 직접 연결되어 있지 않더라도 IXP와 다른 ISP를 거쳐 어떤 위치에 있는 호스트더라도 데이터를 주고받을 수 있는 것이죠.

#### 전국 단위

ISP들을 연결해주는 IXP는 하나의 ISP에서 나온 트래픽을 다른 ISP에 전달하는 역할을 합니다. ISP에서 나온 모든 트래픽이 IXP를 거쳐가야 하므로 IXP에는 수많은 스위치들이 존재합니다.

`peering link`는 일종의 맞춤 서비스입니다. 특정 라우터(또는 액세스 네트워크)들 간의 트래픽이 많을 경우 peering link를 설치하여 일종의 프리미엄 서비스를 제공할수도 있습니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213184728.png" alt="image-20230213184728552" style="zoom:50%;" />

#### 지역 단위

`Regional ISP`는 전국 규모의 ISP를 거치기 전에 지역적으로 서비스를 제공하는 계층에 해당합니다. 

우리나라의 경우 땅이 좁기 때문에 KT나 SKT와 같은 ISP들이 지역 서비스와 전국 서비스를 모두 운영하고 있지만, 미국처럼 땅이 큰 경우에는 지역 ISP와 전국 ISP 가 명확히 구분되는 편입니다.



### Content Provider Network

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213204005.png" alt="image-20230213204005335" style="zoom:50%;" />

CDN(Content Distribution Network)이라고도 불리는 Content Provider Network는 대규모 서비스를 제공하는 기업에서 각 지역에 신속하게 서비스를 제공하기 위해 운영하는 네트워크입니다.

구글이 동아시아 지역을 대상으로 하는 CPN을 운영하는 것이 하나의 예시가 되겠습니다.

CPN은 ISP와 별도의 네트워크를 가지며 그림처럼 Tier 1 계층에서 국가 단위의 서비스를 제공합니다.

ISP와 CPN의 계층은 정형화된 구조가 아니기에 Regional ISP를 거치지 않고 구글의 서비스를 이용하게 될수도 있습니다.



## Performance: loss, delay, throughput

### Packet delay의 4요소

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230213210906.png" alt="image-20230213210906373" style="zoom: 50%;" />

패킷의 총 전송시간은 nodal processing 시간 + queueing 시간 + transmission 시간 + propagation 시간 입니다.

> 여기서 언급하는 node는 라우터에 해당합니다.

### Nodal Processing

nodal processing 단계에서는 비트에러가 발생했는지를 검사한 뒤, line card가 forwarding table 자료구조를 통해 output link를 결정하고 이를 switching fabric 이라는 내부 네트워크를 통해 이동시킵니다.

L3 패킷의 Header에 있는 checksum을 확인했을때 invalid하다고 판정이 난다면 패킷을 폐기합니다.

이 단계는 일반적으로 ms 미만의 적은 시간이 걸립니다. 

proc = 비트에러 체크, 아웃풋 링크 결정 (ms 미만, line card가 forwarding table을 참조하여 계산)

### Queueing

큐잉은 말 그대로 패킷이 대기하는 시간에 해당하며, 패킷이 들어오는 지점(input link)과 패킷이 나가는 지점(output link) 모두에서 발생할 수 있습니다.

하나의 input link로 많은 패킷이 들어온다면 들어오는 쪽에서 딜레이가 발생할 것이고, 라우터의 line card들이 하나의 output link로 패킷을 보내려 한다면, 나가는 쪽에서의 딜레이가 발생할 것입니다.

### Transmission

Transmission delay는 패킷을 밀어넣는데 걸리는 시간입니다. 이전 정리글에서 언급했던 수도관에 물을 밀어넣는데 걸리는 지연시간이 transmission delay에 해당합니다. 대역폭에 의해 지연 시간이 달라지게 됩니다.

### Propagation

Propagation은 라우터로부터 빠져나온 하나의 bit가 다음 라우터에 도착하는데 걸리는 시간입니다. 물리적 전달 매체의 특성과 실제 거리에 영향을 받는 요소입니다.

<br/> 

위의 네가지 요소들이 합쳐져 최종적인 패킷 딜레이 시간이 나오게 됩니다.