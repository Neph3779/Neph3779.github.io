---
layout: post
title: "[컴퓨터 네트워크] #2 Introduction - 2"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Access networks

액세스 네트워크는 엔드 시스템을 엣지 라우터에 연결하기 위해 구축된 네트워크들입니다.

이들은 크게 홈 네트워크, 무선 네트워크, 기업 네트워크로 구분됩니다.



### 액세스 네트워크 - 홈 네트워크

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210155559.png" alt="image-20230210155558951" style="zoom: 33%;" />

홈 네트워크는 ISP의 네트워크로부터 다양한 기기들을 연결하는 구조로 이루어져 있습니다.

와이파이 라우터(와이파이 액세스 포인트)의 경우 라우터에 유선으로 연결되어있으며

와이파이 라우터는 무선기기들을 네트워크에 연결시켜주는 역할을 합니다.

장치를 직접 라우터에 유선으로 연결할수도 있으며 라우터의 L2 기술로는 Ethernet을 사용합니다.

> Wifi Access Point는 줄여서 Wifi AP라고 부릅니다.



### 액세스 네트워크 - 무선 네트워크

| ![image-20230210155627225](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210155627.png) | ![image-20230210155635340](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210155635.png) |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                            무선랜                            |                          이동통신망                          |

무선 네트워크는 베이스 스테이션(4G, 5G의 기지국) / 액세스 포인트(Wifi AP)를 통해 엔드 시스템에 연결됩니다.

무선 네트워크는 무선랜(WLANs)과 이동통신망으로 구분할 수 있습니다.

1. Wireless local area network (WLANs 무선랜)
   - 802.11 기술(aka Wifi)을 사용합니다.
   - 802는 Ethernet에 대한 표준이며 .11은 무선 기술에 대한 표준입니다. (표준은 IEEE에서 제작되었습니다.)
2. Wide-area cellular access network (이동통신망)
   - Kt, Skt 등에서 제공하는 망입니다. (LTE, 5G 등)
   - 일반적으로 모바일 환경에서 접근하게 됩니다.



### 액세스 네트워크 - Enterprise 네트워크

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210155805.png" alt="image-20230210155805316" style="zoom:50%;" />

학교나 회사와 같은 기관에서 사용하는 네트워크인 Enterprise 네트워크는

기관 외부로부터의 접근 / 기관 외부로의 요청을 라우터로부터 전달받아, 이더넷 스위치를 통해 각 기기에 제공합니다.

Wifi 망을 구축하는 방식에는 ISP에 Wifi망을 깔아주길 요청하는 방법과 직접 망을 구축하는 두가지 방법이 있습니다.



### 액세스 네트워크 - 데이터 센터 네트워크

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210160339.png" alt="image-20230210160339700" style="zoom: 50%;" />

규모가 큰 IT 회사들은 대부분 데이터 센터 네트워크를 운영하고 있습니다.

빨간 네모 박스가 수십대의 컴퓨터에 해당하며 데이터 센터에는 수많은 컴퓨터들이 연결되어 있습니다.

한 건물에 데이터를 관리할 컴퓨터를 잔뜩 모아놓은 형태를 띄고 있습니다.



## 패킷 전송 (Host의 관점)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210161430.png" alt="image-20230210161430123" style="zoom:50%;" />

`패킷`은 데이터의 전송 단위입니다. 

어플리케이션이 주고 받는 메세지를 적절한 단위로 나누었을때 나누어진 한 조각이 바로 패킷에 해당합니다.

어플리케이션이 메세지를 보낼때 호스트는 메세지를 받아 *L* bits 단위로 잘라 패킷으로 만든 뒤

이를 물리적 매체를 통해 액세스 네트워크에 전달합니다. (액세스 네트워크는 호스트가 인터넷과 연결되는 최초의 통로입니다.)

이때 물리적 매체의 전송률인 *R* 을 안다면 패킷을 전송하는데 걸리는 시간(packet transmission delay)을 다음과 같이 계산해볼 수 있습니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210161628.png" alt="image-20230210161628240" style="zoom:50%;" />

수도관에 비유를 하자면 단면적이 큰 수도관이 단면적이 작은 수도관에 비해 물을 더 많이 흘려보낼 수 있는것처럼, 대역폭이 클수록 더 많은 데이터를 한번에 보낼 수 있습니다. 즉, 단위시간당 데이터 처리량은 R (대역폭)에 비례합니다.

>  Link transmission rate, Link capacity, Link bandwidth(대역폭)은 모두 물리적 매체의 전송률(R)을 나타내는 말입니다.



## Network Core

라우터들의 그물망 같은 연결을 `네트워크 코어`라고 합니다.

라우터들의 연결 방식에는 규칙이 없기에 마치 무작위로 연결된 것처럼 보이기도 합니다.

<br/> 

패킷을 목적지 호스트로 이동시키기 위해선 네트워크 코어를 거쳐야하는데

이때 패킷을 목적지로 전달하기 위해 사용하는 기술 중 하나가 바로 `패킷 스위칭`입니다.

> Internet에서는 패킷 스위칭 기술을 사용하고 있으며 이와 대조되는 기술로는 Circuit 스위칭이 있습니다.



### PC와 IP주소

> Q. 하나의 PC가 두 개 이상의 IP 주소를 가질 수 있나요?

인터넷을 이용하기 위한 IP 주소는 pc에 꽂혀있는 네트워크 인터페이스 카드에 부여되기 때문에

하나의 pc가 여러개의 네트워크 인터페이스 카드를 사용한다면 얼마든지 가능합니다.

이를 활용해 PC를 라우터 역할로 사용할수도 있습니다. (리눅스를 설치하여 운영할 수 있습니다.)

하지만 보통은 인터페이스 카드를 pc 한 대당 하나만 꽂기 때문에 일반적으로 pc 한 대당 하나의 ip주소를 가지는 것처럼 보이는 것이죠.



### Router

라우터는 컴퓨터입니다. 다만 특수한 일을 하도록 만들어진 컴퓨터일 뿐입니다.

라우터는 여러개의 랜카드를 장착하고 있습니다. 그러므로 라우터에는 ip주소가 여러개 부여가 되어 있습니다.

각각의 랜카드에는 라인 카드라고 하는 보드(일종의 컴퓨터)가 들어가 있는데

이 라인카드들을 연결시켜주는 네트워크가 라우터 안에 들어가 있습니다.

이 네트워크를 `Switching Fabric`이라고 합니다.

라인 카드는 들어온 패킷이 어떤 아웃풋 포트로 나가야 할지를 결정하게 됩니다. 

이 과정을 `forwarding`혹은 `switching`이라고 부릅니다.



### Routing vs Forwarding

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210165901.png" alt="image-20230210165901698" style="zoom:33%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210165912.png" alt="image-20230210165912833" style="zoom:33%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                           Routing                            |                          Forwarding                          |

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230210170050.png" alt="image-20230210170050740" style="zoom:33%;" />

Forwarding은 인풋 링크를 통해 들어온 패킷을 어떠한 아웃풋 링크로 보내야될지를 결정하고 보내는 작업입니다.

이때 사용하는 자료구조가 바로 forwarding table이며, 각각의 라인 카드는 forwarding table을 지니고 있습니다.

<br/>

Forwarding은 한 라우터가 독자적으로 목적지를 결정하는 것이므로 **local한 액션**입니다.

이에 반해 Routing은 Source로부터 Destination으로 가는 길을 찾아주는 역할을 하니 **global한 액션**입니다.

각 라우터가 적절하게 포워딩을 해야만 적절한 라우팅이 이루어지기 때문에 포워딩 테이블은 라우팅 알고리즘을 이용해 만들어집니다. 이때 라우팅 알고리즘은 모든 라우터들이 일관성을 가지고 목적지에 보낼 수 있도록 제작됩니다.
