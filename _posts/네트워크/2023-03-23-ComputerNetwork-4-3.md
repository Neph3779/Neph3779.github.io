---
layout: post
title: "[컴퓨터 네트워크] #21 Network Layer - 3"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Generalized forwarding

기존의 router는 목적지 ip를 보고 longest prefix matching을 통해 포워딩하는것이 전부였습니다.

이런 단순한 작업보다 더 범용적인 작업들을 실행시켜보자는 아이디어에서 등장한 것이 Generalized forwarding입니다.

이전에 언급했던 ECN(Explicit Congestion Notification) 등이 이에 속합니다.

목적지 ip뿐만 아닌 다른 header 필드들을 참조할 수 있으며 (matching)

forward뿐만 아니라 drop이나 copy modify log packet등 다양한 작업들을 수행할 수 있습니다. (action)



### Flow table abstraction

기존의 forwarding table은 목적지 ip주소를 매칭하여 어디로 포워딩할지에 대한 내용만 적혀있었다면, flow table에는 match와 action이라는 필드로 구분됩니다.

flow에 대한 정의는 내려져있지 않으며, 사용자가 알맞게 응용해서 사용하도록 유도하고 있습니다.

단, 여러 헤더를 조합해서 내용을 만들 수 있다는 점은 변하지 않습니다.

이때 match에 참여할 헤더는 다른 계층의 헤더도 참여시킬 수 있으며

match 조건들을 다양한 논리연산을 통해 더욱 다양한 조합을 제작할 수 있습니다.

또한 match rule 마다 priority를 다르게 두어서 여러개의 match가 발생했을때 어떤 action을 취할건지를 결정할 수도 있습니다.

counters라는 필드에는 매칭에 사용되는 byte, packet 수를 저장하며, 이는 통계를 위한 필드입니다.



### OpenFlow: flow table entries

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322213139.png" alt="image-20230322213139482" style="zoom: 33%;" />

위의 그림은 open flow라는 프로토콜이 정의한 flow table의 형태입니다.

match, action, status라는 세가지의 필드로 구분되며 여기서 status는 통계에 해당합니다.



매칭할 수 있는 헤더 필드에는 Link, Network, Transport 계층의 헤더가 있으며,

Ingress port는 들어오는 포트를 의미합니다.

> 나가는 포트는 egress port라 부릅니다.



위의 프로토콜을 추상화하면 아래와 같은 구현도 가능해집니다.

|          | match                    | action                   |
| -------- | ------------------------ | ------------------------ |
| Router   | longest 목적지 IP prefix | forward                  |
| Switch   | destination MAC address  | forward / flood          |
| Firewall | IP address, port#        | permit / deny            |
| NAT      | IP address, port#        | rewrite address and port |

Router는 참조할 수 있는 필드 중에 목적지의 ip를 참조하면 되고

Switch는 목적지 MAC 주소를 참조하면 되는 등

OpenFlow 프로토콜을 사용하면서도 다양한 기기와 호환될 수 있습니다.

> Switch의 action 중 flood란
>
> 이더넷 스위치의 브로드캐스트 기능 수행시
>
> 처음으로 네트워크에 접속하는 장치는 ip주소를 누구에게 할당 받아야 할지에 대해 모르기에
>
> 아무나 나에게 ip주소를 할당할 수 있도록 해달라고 광고를 하는 것이 flood에 해당합니다.

<br/>

이렇듯 다양한 매치 옵션과 다양한 액션을 통해 네트워크를 프로그래밍할 수 있도록 만드려는 시도가 진행되어왔고, 현재는 이보다 더 발전된 형태들이 많아졌습니다.

### Remote Controller

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230323002420.png" alt="image-20230323002420867" style="zoom:50%;" />

기존의 라우터들은 분산 데이터베이스의 역할을 하며 라우터들끼리 정보를 주고받으며 table을 만들어 나갔습니다.

네트워킹 장치이다보니 table의 내용이나 알고리즘이 모두 동일할 수 없었는데, 이러다보니 비효율적인 동선으로 포워딩을 하는 등의 문제점들이 존재했었습니다.

이런 문제점들을 해결하고자 OpenFlow controller는 하나의 remote controller가 router들에게 동일한 table을 제공하여 위의 문제상황들을 해결합니다.



## Middlebox

미들박스는 네트워크 내부에 존재하면서 기존의 라우터랑 다른 기능을 하는 장치들을 부르는 말입니다.

source와 dest 사이에 존재해야 한다는 특징을 지니며, NAT나 Firewall이 대표적인 미들박스입니다.

이 외에도 IDS(Intrusion Detection System) 같은 보안장치나 Data center에서 주로 사용하는 Load Balancer와 같이 들어오는 패킷들을 적절히 분배해주는 기기 들이 미들박스에 해당합니다.

초기의 미들박스는 하드웨어적 솔루션으로 제공됐었습니다. 회사 자체의 알고리즘, 프로토콜 사용했었고 자연스럽게 호환성이 좋지 못하다는 문제가 있었습니다.

요즘에는 Open API를 사용하는 추세로 변해 호환성 문제는 상당 부분 해결되었습니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
