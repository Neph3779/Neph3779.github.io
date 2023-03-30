---
layout: post
title: "[컴퓨터 네트워크] #22 Network Layer - 4"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Routing protocol

라우팅 프로토콜의 목표는 혼잡이 가장 덜한(가장 비용이 적은) 경로로의 안내입니다.

path는 출발지부터 목적지로 이동하며 거쳐가는 라우터를 순서대로 나열한 것을 의미하며, route와 동의어입니다.

좋은 경로의 조건으로는 least cost(실제 부과되는 비용), fastest, least congested 등을 고려해볼 수 있습니다.

router들의 연결은 양방향 weighted graph로 나타낼 수 있습니다. 이때 가중치는 network operator라는 router domain의 네트워크 관리자가 bandwidth, congestion등의 요소를 통해 결정하게 됩니다.

이때 방향에 따라 같은 연결통로더라도 가중치가 다르게 책정될 수 있습니다.

### Routing algorithm

Routing algorithm은 크게 Link state 방식과 Distance vector 방식으로 분류됩니다.

Link state 방식의 대표적인 프로토콜로는 OSPF가 있으며, Distance vector 방식의 예로는 RIP 프로토콜이 있습니다.

### Link state

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230323235612.png" alt="image-20230323235612131" style="zoom: 33%;" />

링크 스테이트 방식은 모든 라우터가 네트워크가 어떻게 구성되어있는지에 대한 정보를 알고 있습니다.

즉, 그래프가 어떻게 구성되어있는지를 알고 있다는 의미이죠.

각각의 노드는 자신이 알고있는 정보를 토대로 table을 제작하며, 이때 사용되는 알고리즘은 우리에게 익숙한 다익스트라 알고리즙입니다.

이웃 노드가 새롭게 갱신된 정보를 알려왔다면, 해당 정보를 통해 나의 table을 갱신하는 방식을 채택합니다.

다익스트라 알고리즘의 특성상 cycle이 없는 least cost path tree (minimum spanning tree)를 제작할 수 있으며, 이때 predecessor를 기록해놓습니다. (a부터 z까지의 최소비용 경로가 있다고 했을때 z에 도착하기 직전에 도착하는 노드에 대한 정보)

초기의 정보는 알고리즘을 시작하는 라우터와 직접연결된 라우터들에 대한 비용 정보이며, 방문하지 않은 라우터들을 하나씩 방문하며 테이블을 채워나가는 형태를 띕니다.

#### Complexity

time complexity는 잘 알다시피 O(nlogn)이며 (priority-heap 사용시)

message complexity는 O(E*N)입니다.

complete graph(최악의 경우)의 경우에는 Edge의 개수가 Node의 개수의 2승이므로

최악의 공간복잡도는 O(N^3)이지만, 보통은 O(N^2)의 공간복잡도를 지닙니다.



#### Oscillation problem

oscillation 문제란 갱신된 정보를 바탕으로 최선의 경로를 선택했더니 그 사이에 다른 노드들의 정보도 업데이트 되어서 나의 table이 업데이트가 완료된 시점에는 최선의 경로가 아니게 되는 문제입니다.

마치 내가 지나가는 차선이 막혀 차선변경을 하였더니 다른 차들도 똑같이 차선을 변경하게 되어서 차가 계속 밀리는 상황과 유사합니다.

라우터는 아주 조금의 이득을 위해서 경로를 매번 변경하게 되는데, 이것이 오히려 도착지에 패킷이 순서대로 도착하지 않는 것을 부추겨서 오버헤드가 더 심해질 수도 있다는 문제를 가집니다.



### Distance vector

Distance vector는 Dynamic programming의 일종인 벨만포드 알고리즘에 의해 구현됩니다.

가령 a부터 z까지의 비용을 구한다면 (a와 b는 직접연결 되어있다고 가정하겠습니다.)

a to b의 비용 + min(b to z의 비용들) 

위와 같은 수식을 통해 a to z의 최소비용을 구하는 알고리즘입니다.

이때 a는 b로부터 전달받은 b to z의 비용들에 대한 정보를 전적으로 신뢰합니다.

이로 인해 해커가 잘못된 정보를 flood해버리거나 router가 오류 등으로 인해 잘못된 경로 정보를 제공해주면 알고리즘이 제대로 동작하지 않는 문제를 가집니다.

Link state 방식에서는 N번의 순회를 통해 모든 정보를 갱신할 수 있었지만, Distance vector 방식에서는 각 노드가 이웃 노드에게 계속해서 정보를 제공하고, 이를 통해 정보를 업데이트 하는 과정이 계속됩니다.

수학적으로는 내가 가진 경로의 정보가 변경되었을때만 이웃 노드에게 notify하는 것이 맞지만, RIP 프로토콜에서는 alive 신호를 보내기 위해 30초마다 정보를 notify합니다.



### Link state vs Distance vector

|                        | link state                       | distance vector                                              |
| ---------------------- | :------------------------------- | ------------------------------------------------------------ |
| 정보를 전달하는 노드들 | 모든 노드                        | 이웃 노드                                                    |
| 전달하는 정보          | 자신과 지접 연결된 링크들의 정보 | distance vector (여러개의 값), 모든 목적지로 가는 distance 전체의 값 |



## intra/inter-AS routing

지금까지 다룬 내용들은 라우터에 대한 이상적인 내용들이었습니다.

모든 라우터가 동등하고, 네트워크가 flat하다고 가정하고 설명을 했지만, 이는 실제 인터넷 상에서는 적용되지 않습니다.

라우터, 네트워크마다 power와 역할의 차이가 존재하기 때문입니다.

단순한 예로 우리나라와 브라질의 한 router 사이의 link state 알고리즘을 적용하기 위해선 어마어마한 거리를 이동해야하며, 전세계의 모든 router들끼리 정보를 주고받기란 불가능에 가깝습니다.

이러한 문제로 routing 알고리즘을 적용하는 영역을 제한하고 영역끼리의 routing을 담당하는 또다른 routing 방식을 두는 방향으로 발전했습니다.

이 영역의 단위를 AS (Autonomous System)라고 부르며 전세계는 수많은 AS들의 조합들로 이루어져 있습니다.

AS내부의 라우팅 방식은 AS가 마음대로 설정할 수 있으며, AS끼리의 라우팅을 위해 존재하는 프로토콜을 통해 자신의 AS 범위 밖의 router와 연결됩니다.



### intra-AS routing

intra-AS routing이란 AS 내부에서 동작하는 라우팅 방식입니다.

위에서 설명한 설명한 Link State 방식을 주로 사용하며, AS 내부의 라우터들은 모두 동일한 라우팅 알고리즘을 수행합니다.



### inter-AS routing

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230323235519.png" alt="image-20230323235519897" style="zoom:50%;" />

inter-AS routing이란 AS끼리의 라우팅 방식으로, gateway router를 통해 다른 AS와 연결되는 것을 말합니다.

AS끼리 사용할 프로토콜은 모두가 합의해야하는 내용이므로 사실상 표준이라고 볼 수 있는 BGP 프로토콜을 사용합니다.

AS의 가장 최외곽에 붙어있는 router를 통해 다른 AS에게 내 AS의 최소비용 경로를 공유하고 공유받습니다.



### OSPF

OSPF는 가장 널리 사용되는 link state 방식을 사용하는 intra-AS routing protocol입니다.

flooding시에는 TCP/UDP를 사용하지 않고 Ip프로토콜을 사용하며 edge의 cost 산정 시에는 bandwidth나 delay를 각각 또는 혼합하여 사용합니다.

암호화를 통해 routing information을 제공한 router를 검증하는 절차를 거치고, 정보가 도중에 변경되지 않았는지에 대해 체크하는 등의 방법을 통해 보안 측면에서의 기능을 제공합니다.



### 계층적 OSPF

![image-20230323234943987](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230323234944.png)

OSPF를 backbone과 area로 한번더 영역을 구분하기도 합니다.

link state가 모든 노드에 대한 정보를 알아야 하는 것이 부담이니, 이 부담을 덜고자 flooding하는 영역을 제한하자는 아이디어에서 등장한 기법입니다.

backbone은 일종의 gateway가 되어 각각의 area들을 연결해주며, backbone 밖으로 나가는 패킷에 대해서는 backbone의 gateway router가 이를 담당합니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
