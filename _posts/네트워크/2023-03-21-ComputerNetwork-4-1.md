---
layout: post
title: "[컴퓨터 네트워크] #19 Network Layer - 1"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Network Layer

네트워크 레이어의 장치인 router는 input ports, switching fabric, output ports로 구성되어 있습니다.

네트워크 레이어의 동작은 지역성에 따라 data plane과 control plane으로 구분합니다.

지역적인 특성을 지닌 data plane은 들어온 input을 어떤 ouput으로 내보낼지에 대한 결정이며, 이 과정을 forwarding이라고 부릅니다.

network-wide 특성을 지닌 control plane은 source부터 destination까지 어떻게 찾아갈지에 대한 결정을 하며 이 과정을 routing이라고 부릅니다.

### Control plane

| ![image-20230321163333981](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321163334.png) | ![image-20230321163349406](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321163349.png) |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                   Per-router control plane                   |                      SDN control plane                       |

control plane에 적용되는 알고리즘은 전통적 routing 알고리즘(per-router)과 SDN(software-defined networking)이 있습니다.

각각의 라우터가 정보를 공유하여 forwarding table을 만들던 전통적 방법과 다르게 SDN은 라우팅 알고리즘을 서버에 저장해서 이를 모든 라우터에 전송하는 기법입니다.

전통적 방법에서는 라우터들끼리 데이터를 주고받으면서 분산알고리즘을 적용하고, 각각이 노드가 되어 알고리즘을 수행합니다.

알고리즘의 수행결과인 포워딩 테이블은 각각의 노드에 저장되며 이를 통해 어떤 output port로 내보낼지를 결정합니다.

SDN은 remote controller를 통해 포워딩 테이블을 내려받으며, 컨트롤러의 결정에 따라 포워딩 테이블의 내용이 바뀔 수 있습니다.



### Ip protocol

ip protocol은 성공적인 전달, 전달되는 타이밍과 순서보장, 보장된 크기의 대역폭의 기능을 모두 제공하지 못합니다. 

그저 best effort를 통해 서비스를 할 뿐이죠.

이런 상황에서 서비스의 품질을 보장하는 것이 불가능해보이지만, 대역폭이 폭발적으로 발전하면서 best effort만 가지고도 서비스를 제공하는 것에 문제가 없었기에 다른 프로토콜이 주가 되지 못하고 있습니다.

특히 CDN의 공급으로 인해 사용자와 서비스 제공자 사이의 거리가 줄어들었고, congestion이 발생하더라도 서비스 측면에서 유연하게 대처하는 서비스들이 늘어났기에 더더욱 ip 프로토콜만이 살아남게 되었습니다.

> 실제로 Ip 프로토콜을 대신하기 위해 Diff serv, Int serv 등이 등장했지만 모두 흐지부지 되었습니다.



## Router의 구조

### 개요

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321151039.png" alt="image-20230321151039782" style="zoom: 33%;" />

라우터는 input port, switching fabric, ouput port로 구성됩니다.

routing을 control plane 담당하는 영역과 forwarding을 담당하는 data plane 영역으로 구분지어볼 수 있습니다.

### Input port

routing processor가 라우팅 알고리즘을 실행시켜 얻어진 자료구조를 input port와 ouput port의 line card에 뿌려줍니다.

routing processor는 header의 목적지 ip 주소를 보고 forwarding table을 참조해서 이를 ouput port로 내보냅니다.

destination-based forwarding 방법에서는 목적지 ip주소에 근거해서 forwarding table을 참조하며

generalized forwarding이란 여러가지 필드를 참조하는 방식이지만 대부분 destination-based forwarding으로 서비스를 제공하고 있습니다.

### Longest prefix matching

forwarding 테이블에는 ip주소의 앞부분을 매칭시켰을때 최대로 매칭되는 값으로 forwarding을 진행합니다.

 만약 111000111 이라는 주소가 들어왔고 테이블에 `111000***` 이라는 key값과 `1110*****`이라는 key 값이 존재한다면 전자의 key값에 적혀있는 output port로 나가게 되는 것이죠.

여기서 *는 와일드 카드라 부르며, 어떠한 값이 들어와도 괜찮다는 의미를 가집니다.

이 방법을 Longest prefix matching이라 부르며, Ternary content addressable memories라는 하드웨어를 통해 one clock cycle에 처리합니다.

해당 하드웨어는 만들기 힘들고 전력소모가 심하다는 특징을 가지고 있습니다.

> ternary는 3진수를 의미합니다. (0, 1, *)



### Switching fabric

Switching fabric은 패킷을 신속하게 아웃풋 포트로 내보내는 역할을 합니다.

Switching rate를 통해 성능을 평가하며 switching을 구현하는 방식은 크게 메모리, Bus, Interconnection network의 형태로 나뉩니다.

보통 스위치라고 하면 L2 계층의 이더넷 스위치를 의미하지만 router는 switch 중에서 ip packet을 스위칭하는 목적의 L3 switch라고 말할 수 있습니다. 즉, 라우터도 엄밀히는 스위치에 포함되는 것이죠.



#### 메모리

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321151906.png" alt="image-20230321151905993" style="zoom:50%;" />

메모리 형태는 초창기에 사용되던 방식이며 현재는 거의 사용되지 않습니다.

단순히 컴퓨터에 랜 카드 두개를 붙인 것과 같게 동작하며

랜카드로 들어온 input을 메모리가 다시 output 랜카드로 내보내주는 형태입니다.

이때 메모리에 썼다가 다시 그 메모리에 쓴 내용을 읽어가야하기에 좋은 성능을 내기 힘든 구조입니다.



#### Bus

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321151917.png" alt="image-20230321151917638" style="zoom:50%;" />

Bus 형태는 하나의 버스를 통해 input이 ouput으로 내보내지는 구조입니다.

Bus contention(버스를 잡기 위한 경쟁)이 발생하며 버스의 대역폭에 따라 처리 속도에 제한이 생깁니다.

비교적 소형급 라우터에 사용되는 형태이며 고성능 라우터에는 사용되지 않습니다.



#### Interconnection network

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321151942.png" alt="image-20230321151942361" style="zoom:50%;" />

interconnection network란 여러개의 네트워크로 구성된 하나의 네트워크로

interconnection을 이루고 있는 보드가 여러개 존재하여, 여러개의 interconnection을 통해 빠르게 포워딩하는 전략입니다.

여러개의 스위치를 모아 하나의 큰 스위치를 구성하는 multistage switch를 사용하기도 합니다.

그림의 crossbar형태의 interconnection 네트워크 외에도 다양한 형태가 존재할 수 있습니다.



### Input port queuing

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321213610.png" alt="image-20230321213610062" style="zoom:50%;" />

packet 처리 속도보다 들어오는 packet이 많다면 queuing이 발생합니다.

버퍼가 가득 찬 상태에서 들어오는 패킷은 로스가 발생하며

그림과 같은 상황에서 초록색 datagram은 빨간색 output port의 처리로 인해 초록색 output port가 비어있음에도 내보낼 수 없는 일종의 HOL Blocking을 겪게 됩니다.



### Output port queuing

packet들이 link transmission rate보다 빨리 도착하면 output port에서도 대기열이 생깁니다.

만약 버퍼가 가득찬 상태에서 들어오면 packet을 drop할 수 밖에 없습니다.



### Buffer 관리

Buffer관리에는 drop시에 어떤 packet을 drop할 것인지와

패킷을 내보낼때 어떤 패킷을 먼저 내보낼지에 대한 결정이 존재합니다.

대부분의 경우 큐의 tail을 drop하고, FCFS 방식으로 패킷을 내보내겠지만

priority를 두어 drop/패킷 내보내기를 결정할수도 있습니다.

이 외에도 congestion을 알리기 위해서 어떤 패킷에 marking을 할 것인지 등의 내용이 있을 수 있습니다.

### Packet Scheduling

패킷들을 내보내기 위한 스케쥴링 기법은 운영체제에서 process scheduling기법들과 유사한 기법들이 사용됩니다.

FCFS, priority queue, round robin 등의 익숙한 방법을 통해 진행할 수도 있으며

weigthed fair queueing이라는 기법을 통해 데이터를 더 많이 보낸 사용자의 데이터 처리가 활발히 이루어질 수 있도록 할 수도 있습니다.



## Internet Protocol(IP)

네트워크 계층은 3계층에 속하며 가장 중요한 프로토콜은 IP(Internet Protocol)입니다.

이 외의 3계층 프로토콜로는 라우팅의 control plane에서 사용되는 routing protocol,

ping, trace route 등의 프로그램에서 라우팅 도중 특이사항이나 오류가 발생했을때 라우터가 source에게 그 내용을 전달하기 위해 사용되는 ICMP(Internet Control Message Protocol) 등이 존재합니다.



### IP Datagram format(IPV4)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321233921.png" alt="image-20230321233921300" style="zoom:50%;" />

- version 필드는 현재 IP버전이 실리는 필드입니다.

- header length는 option에 의해 헤더의 길이가 늘어날 수 있기에 사용됩니다.
- type of service 필드는 패킷의 우선순위를 결정할때 사용되지만 거의 쓰이지 않습니다.
- length는 데이터의 길이를 나타냅니다.
- identifier와 fragmentation offset은 하나의 패킷이 지나가는 switch의 한계로 인해 한번에 전송하지 못하고 나누어 전송해야 할 때, 이를 쪼개게 되므로 필요한 필드입니다.
  - 쪼갤때 각각의 쪼갠 단위에 ip header를 붙이며, 이때 몇번째 조각인지를 offset에, 각각의 조각이 원래 하나였다는 것을 알기 위해 id가 사용됩니다.
- time to live는 trace route 프로그램을 설명할때 언급했던 것처럼, 하나의 라우터를 지나갈때마다 1씩 감소되며 0이 되면 더이상 포워딩시키지 않고 오류가 발생했음을 알립니다.
- upper layer는 TCP를 쓰는지, UDP를 쓰는지에 대한 정보가 담겨있습니다. (4계층 프로토콜을 어떤 것을 쓰는지에 대한 정보)
- header checksum은 UDP를 설명하며 언급했던 checksum과 같은 기능입니다.
  - 대부분 TLS같은 보안 프로토콜을 통해 데이터 훼손 여부를 판단하므로 큰 의미를 지니는 필드는 아닙니다.
- Source IP 주소, Destination IP 주소
  - IP는 host to host 통신을 책임지기에 해당 정보들이 가장 중요한 요소들이라 할 수 있습니다.

### IP addressing

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230321234419.png" alt="image-20230321234419705" style="zoom:50%;" />

IP 주소는 dotted decimal 형태를 띕니다.

이는 32bit의 주소를 8bit씩 쪼개서 쪼갠 각각을 10진수로 표현한 뒤, 이를 .으로 이어붙인 형태입니다.

Ip주소는 pc의 네트워크 interface에 부여되며 interface란 router나 host를 처음으로 연결하는 물리적인 link를 뜻합니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
