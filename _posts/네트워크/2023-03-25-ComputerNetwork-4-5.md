---
layout: post
title: "[컴퓨터 네트워크] #23 Network Layer - 5 (完)"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Inter-AS routing: BGP

BGP는 일종의 인터넷을 연결시켜주는 접착제입니다.

이전에 ISP는 광고를 통해 특정 ip주소 대역의 트래픽을 자신에게 전달해주기를 요청한다고 했었는데 이를 가능케 해주는 기술이 바로 BGP입니다.

BGP는 AS간의 통신을 위한 eBGP(exterior)와 다른 AS로의 이동정보를 내부에 공유하기 위해 사용되는 iBGP(interior) 두개의 세부 프로토콜로 구분됩니다.

OSPF와 같은 intra-AS routing 프로토콜은 성능적인 측면이 강조되었었지만, BGP는 성능보다는 정책을 우선시합니다.

가령 나를 지나가는 트래픽이 나의 고객이 보내는 트래픽이 아니라면 이를 통과시켜주는 것은 성능적 낭비이기에 나를 지나가는 루트가 최선의 루트라 하더라도 이를 우선적으로 막을 필요가 있습니다. 



### BGP connection

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230324172435.png" alt="image-20230324172435921" style="zoom:50%;" />

하나의 AS 내의 라우터들은 모두 논리적으로 연결되어 있습니다. (직접 연결되어있지 않더라도 다른 라우터를 경유해서 도달할 수 있다는 의미와도 같습니다.)

붉은 원으로 감싸진 라우터들은 gateway router로, eBGP와 iBGP를 동시에 수행합니다.

BGP는 session을 통해 연결되는데, OSPF의 연결에서 IP 프로토콜을 사용한것에 반해 BGP에서는 TCP를 사용합니다.

여기서 연결된 두 gateway router를 peer라고 부르며, 이 peer끼리의 통신을 통해 목적지에 가기 위해서 어떤 AS를 거쳐가야 하는지에 대한 정보를 알게됩니다.

### BGP의 광고

BGP가 광고하는 정보는 prefix와 attributes로 구분됩니다.

prefix는 목적지의 ip주소로 목적지에 대한 정보를 나타내며, attributes에는 AS-PATH라는 거쳐가야 하는 AS들의 순서와, NEXT-HOP이라는 다음으로 거쳐가야 하는 라우터에 대한 정보가 포함됩니다.

가령 위 그림의 AS1이 자신을 거쳐가야 한다는 것을 AS2에게 광고한다면, NEXT-HOP으로 1c 라우터의 정보를 내용에 실어서 보낼 수 있습니다.



### Policy-based routing

위에서도 언급했듯 BGP는 성능보다 정책을 우선시합니다.

다른 AS가 보내온 광고를 무시하거나, 특정 이웃노드에게만 나의 정보를 광고하는 등의 기법을 사용할 수 있습니다.

BGP의 라우팅 우선순위는 다음과 같습니다.

1. policy 
2. shortest AS-PATH
3. closest NEXT-HOP router(hot potato)



### BGP message의 종류

BGP의 메시지의 종류로는 새로운 session을 열기위한 OPEN, 새로운 경로를 광고하기 위한 UPDATE, UPDATE 메시지에 대한 ACK이자 AS가 살아있음을 알리는 KEEPALIVE, 에러 발생이나 TCP 연결을 종료할때 보내는 NOTIFICATION이 있습니다.

대부분의 상황에서 TCP 연결은 반영구적으로 지속되지만, 긴급한 상황일 경우에는 이를 종료하기도 합니다.



### iBGP 

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230324190238.png" alt="image-20230324190238629" style="zoom:50%;" />

AS1의 gateway router인 1c는 2a로부터 X라는 라우터에 도달하기 위해선 AS2와 AS3를 차례로 거쳐가야 한다는 정보를 받고, 3a로부터 AS3를 거쳐가야한다는 정보를 받습니다.

1c는 최적의 경로인 AS3로 가야한다는 정보를 내부의 라우터들에게 알리게 되는데,

AS1을 통과해 AS3로 가기위해서 어떤 라우터를 거쳐가야 하는지에 대한 정보를 1d가 판단할때는 당연하게도 iBGP 메시지를 보낸 1c를 통과하도록 설정합니다.

그림에서 보듯, 1d는 X로 가기 위해 1번 인터페이스로 포워딩하도록 포워딩 테이블을 구성했음을 볼 수 있습니다.



### Hot potato routing

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230324225801.png" alt="image-20230324225801867" style="zoom:50%;" />

Hot potato routing이란 뜨거운 감자를 손에 쥐면 얼른 내 손에서 떠나게 하고 싶어진다는 관용적 표현(?)을 가지고 설명하는 라우팅 방법입니다.

만약 그림에서 AS2가 AS3로 가야하는 트래픽을 만난다면 우리가 생각하기에는 당연히 2c로 포워딩을 하여 AS3로 내보내는 것이 합당합니다.

하지만 AS는 지금 당장 트래픽을 내보내는 것에 최우선점을 맞추며, 2c가 아닌 2a로 포워딩시켜 트래픽을 내보냅니다. (AS1을 통해 AS3로 갈 수 있다는 정보를 알고 있다는 가정입니다.)

즉, AS들은 최선의 경로로 라우팅을 하는 것이 아닌, 자신의 AS 구역내에 들어온 트래픽을 최대한 빠르게 처리하는 것에 중점을 두고 트래픽을 처리하며, 이를 뜨거운 감자를 잡은 사람에 빗대어 표현한 것이 hot potato routing입니다.



## SDN control plane

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230324222413.png" alt="image-20230324222413921" style="zoom:50%;" />

SDN은 Software Defined Networking의 약자로 2000년대 중반쯤부터 연구되어온 기술입니다.

기존의 라우터는 라우팅 프로토콜을 실행하는 것이 다였으며, 이를 해결하기 위해 NAT나 Firewall과 같은 여러 midddlebox들이 등장합니다.

하지만 라우터(스위치)가 오직 라우팅 프로토콜만을 실행하는 것이 아닌, 미들박스에서 수행하는 다양한 일들을 하면 안되는가에 대한 의문이 생겨났고, 이를 소프트웨어적으로 해결하기 위한 움직임들이 시작되었습니다.

이 과정에서 등장한 것 중 하나가 Open Flow입니다.

SDN은 하나의  logically centralized control plane을 두고 각각의 switch들은 Control Agent를 통해 테이블을 전달받습니다. 

기존의 per-router 방식은 분산 알고리즘을 수행하기 때문에 각각의 라우터가 수행의 결과로 얻어낸 포워딩 테이블의 정보가 다른 경우가 있었습니다.

또한 라우터에 들어간 라우팅 프로토콜의 알고리즘만을 수행할 수 있었기에, 갑작스런 정책적 변화로 인해 트래픽이 특정 루트를 통과하는 것을 강제해야 하는 상황이 발생하더라도 이를 처리하기 매우 어려웠습니다.

이러한 문제점을 해결하기 위해 SDN이 등장하여 현재까지 발전해오고 있습니다.



### SDN의 계층



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230324222857.png" alt="image-20230324222857651" style="zoom:50%;" />

SDN을 컴퓨터에 비유하면 SDN에 의해 control되는 switch들의 모임을 device,

SDN controller를 OS,

SDN controller 위해서 수행되는 application들을 system program으로 비유할 수 있습니다.

application에서 특정 스위치가 Firewall의 역할을 수행하도록 만들수도 있고, 트래픽이 몰리는 경우에는 스위치를 Load balancer로 동작하도록 지시할수도 있습니다.

SDN controller는 application의 요구사항을 토대로 switch에 테이블을 제작해서 내려주게 되고, switch는 이 테이블을 수행하여 원하는 동작을 할 수 있도록 만듭니다.



## 마치며

이상으로 한학기동안 수강한 "컴퓨터 네트워크" 과목의 정리를 마쳤습니다.

네트워크 정리글을 쓰며 사용한 모든 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에 존재하며, 출처를 명시하면 누구나 이를 업로드할 수 있도록 출판사측에서 배려해주고 있습니다.

잘못된 정보나 질문이 있으시다면 댓글 혹은 neph3779@gmail.com으로 메일주시면 감사하겠습니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
