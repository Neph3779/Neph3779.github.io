---
layout: post
title: "[컴퓨터 네트워크] #20 Network Layer - 2"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## IP Addressing

### Subnets

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322134101.png" alt="image-20230322134101385" style="zoom: 33%;" />

Subnet은 router를 거치지 않고 도착할 수 있는 네트워크입니다. 이는 그림의 하늘색 영역에 해당합니다.

Ip주소는 subnet part와 host part로 구분되는데, 32bit중 prefix에 해당하는 부분이 subnet part, 그 외의 나머지 부분이 host part에 해당합니다.

subnet part는 subnet들을 구분하는 id이며, 몇번째 비트까지가 subnet 파트에 해당하는지를 표기하기 위해 subnet mask라는 표현방법을 사용합니다.

가령 ip주소 뒤에 /24 라는 문구가 붙어있다면 해당 ip 주소의 앞 24자리는 subnet 파트, 나머지는 host 파트임을 나타냅니다.

라우터와 라우터를 연결하는 영역도 서브넷에 해당하며 이곳의 host part는 모두 0입니다.



### DHCP

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322150736.png" alt="image-20230322150736671" style="zoom:50%;" />

ip주소는 static한 값이 부여될수도 있지만 사용자의 위치에 따라 매번 다른 액세스 네트워크와 연결돼야 하는 모바일 환경에서는 매번 새로운 ip를 부여해야 합니다.

이때 ip를 부여하는 주체는 DHCP서버이며, 이 DHCP 서버를 운영하는 주체는 ISP입니다.

DHCP는 사용자가 네트워크에 join할때 ip를 부여하며 그 과정은 다음과 같습니다.

1. 호스트가 DHCP 서버에 접속하여 자신에게 ip주소를 할당해줄 것을 요청하는 boradcast 메시지를 보냅니다.

2. DHCP 서버는 이에 응답해 ip주소를 제안하는 offer 메시지로 답장합니다.

3. host는 해당 ip주소를 검토한 뒤 사용할 것임을 알리는 request 메시지를 보냅니다.

4. 최종적으로 server는 ack 메시지를 보냄으로써 ip주소가 할당됩니다.

여기서 첫 두단계는 이미 특정 ip주소에 연결됐던 적이 있다면 생략이 가능합니다.

즉, 이전에 할당받았던 ip주소를 서버에 바로 전송하여 사용 가능여부를 체크한 뒤, 서버가 ack 메시지를 보내면 해당 ip주소를 바로 사용하게 되는 형식입니다.

DHCP는 ip주소 할당 외에도 가장 처음 만나는 router의 주소, DNS 서버의 주소, network mask 등을 제공합니다.



## IP 주소 할당

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322151516.png" alt="image-20230322151516170" style="zoom:50%;" />

ISP는 특정한 ip대역을 가지고 있으며, 이를 고객들에게 분배합니다.

그림과 같은 200.23.16.0/20 이라는 ip중 앞 20자리를 제외한 나머지 부분들에 변화를 주어 ip를 할당할 수 있습니다.

만약 8개의 고객사에 ip주소를 분배하고 싶다면 21번째부터 24번째까지의 총 3bit로 8가지 경우의 수를 만들어 분배할 수 있습니다.

<br/>



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322151741.png" alt="image-20230322151741666" style="zoom:50%;" />

이렇게 ip를 할당했으면 해당 ip주소가 destination이 되었을때 알맞게 패킷을 전달해줄 수 있어야 합니다.

ISP들은 자신들의 ip주소와 매칭되는 destination ip로의 요청을 자신들에게 전송해달라고 router들에게 광고합니다.

ISP가 광고하는것은 자신에게 할당된 주소이며 router는 longest prefix matching을 통해 패킷을 전달해줍니다.

ISP로 패킷이 들어오면 이를 다시 고객사들(혹은 고객)에게 알맞게 전달해주는 과정을 거치게 됩니다.



### ISP 이동으로 인한 문제

이전에 ISP를 바꾸더라도 ip주소는 그대로 사용하는 경우들에 대해 언급한 적이 있습니다.

가령 KT에서 SKT로 ISP를 변경했지만 KT와 SKT가 계약을 맺어서, SKT가 KT의 ip주소 영역을 사용하도록 하는 것이죠.

이때 router에게 보내는 광고의 메시지는 누가 변경해야 할까요?

정답은 SKT만 변경하면 된다입니다. Longest prefix matching 기법을 사용하므로 KT는 광고의 내용을 변경하지 않고, SKT만 더 자세한 ip주소를 광고하게되면 해당 ip주소를 dest로 삼는 패킷을 전달받을 수 있게 됩니다.



### ICANN

ISP가 고객들에게 ip주소 대역을 분배하기 전, ISP에게 주소 대역을 분배하는 기관은 ICANN입니다.

ICANN은 Internet Corporation for Assigned Names and Numbers의 약자로, ip주소 대역 분배 외에도 DNS root zone의 관리, TLD에 root zone의 데이터를 전달하는 등의 일을 하는 기관입니다.



## NAT(Network Address Translation)

128bit 주소 체계인 IPv6와 달리 IPv4 주소는 32-bit 주소체계입니다.

전세계 인구가 사용하기에는 너무나 적은 숫자이기에 이미 여분은 거의 다 사라졌습니다.

이런 상황에서도 큰 문제가 없는 이유 중 큰 역할을 하는 것은 아마 NAT일 것입니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322155635.png" alt="image-20230322155635268" style="zoom:50%;" />

NAT는 우리나라에서는 흔히 `공유기`라는 이름으로 불리고 있는 장치입니다.

하나의 ip주소를 공유하여 여러 기기가 사용할 수 있도록 만들어주는 장치이기에 붙여진 이름입니다.

network mask 표기법상에서 10/8, 172.16/12, 192.168/16과 매칭된다면 이는 private ip 주소들입니다.

> 10/8은 10.0.0.0/8을 의미합니다.

외부에 노출되는 ip주소는 하나이지만 이를 private network에서는 여러개로 분할하여 사용할 수 있도록 만들어주며, 이때 private network 내의 device를 구분하기 위해 NAT translation table이 사용됩니다.

### NAT의 동작

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322161522.png" alt="image-20230322161522596" style="zoom: 50%;" />

내부에서 외부로 패킷을 전송할때는 private ip 주소와 port번호로 NAT 장치에 데이터 송신을 요청합니다.

NAT 장치는 이를 NAT장치의 ip주소(public ip 주소)와 해당 요청을 구분하기 위한 포트번호를 table에 할당해놓습니다.

response가 NAT로 들어오면 table을 참조하여 어떤 private 장치로 전송해야할 것인지를 판단하여 알맞은 곳으로 패킷을 보내줍니다.



### NAT의 장점

- 하나의 ip주소로 다수의 device를 연결할 수 있습니다.
- local network에서 host의 주소를 원하는대로 바꿀 수 있습니다.
  - 외부에 알릴 필요가 없습니다.
- ISP를 바꾸더라도 내부의 요소를 변경할 필요가 없습니다.
- 보안 측면에서도 외부에서는 내부에 관해 모르므로 내부에 대한 공격이 가해지기가 어렵다는 장점이 있습니다.



### NAT에 대한 부정적 시선들

처음 NAT가 등장하고나서 굉장한 논란이 있었습니다.

순수하게 학자의 입장에서 봤을때 NAT는 꼼수에 불과하기 때문이죠.

그림에서 보다시피 NAT는 하나의 router처럼 그려져있습니다. Router는 3계층의 장치이니, 3계층까지만을 다루어야 하는데, NAT는 4계층에 해당하는 port번호를 다루고 있다는 점,

이러한 방법을 써서 ip주소 부족 문제를 해결할 바에는 IPv6를 쓰는 게 맞다는 의견,

네트워크 내부 장치는 신속하게 패킷 교환만을 하고, intelligence가 필요한 내용들은 모두 host에게 맡기자는 end-to-end 규칙을 깬다는 점,

NAT 내부에서 서버를 운영한다면 public으로부터의 접근을 엉뚱해보이는 port 번호로 접근해야 한다는 점 

등 다양한 부정적 인식이 있었습니다.

> 가령 웹서버는 80번 포트를 사용해야 하지만, NAT 내부에서 운영되는 서버라면 NAT에 의해 변형된 port 번호로 접근해야 하는 문제입니다.

하지만 일반인의 시선에서는 그저 문제없이 서비스를 받을 수 있다면 문제가 없었기에

큰 관심을 가지지 못했고, 현재 NAT는 광범위하게 사용되고 있습니다.



## IPv6

IPv6가 등장하게 된 배경은 당연하게도 ip주소의 고갈이었습니다.

IPv6가 등장한지는 20년도 넘었으며, 그 당시에는 IPv4에 많은 문제가 있으니

IPv4와의 호환성을 고려할 것이 아니라 모두가 다같이 IPv6를 사용하게 만들 수 있을 것이라 믿었습니다.

하지만 모두가 한날 한시에 IPv6로 전환하는 것은 무모한 생각이었으며, 현재까지도 IPv6는 대세로 자리잡고 있지 못합니다.



### IPv6의 header

![image-20230322163014940](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322163014.png)

IPv6의 header는 option 필드가 없으며, 무조건 40byte를 지킵니다.

flow 라는 라벨을 통해 사람들이 다양한 작업을 하기를 원했으며 (어떻게 활용할지에 대한 정의가 내려져있지 않음)

pri는 priority, payload length는 데이터의 길이, next header는 data의 성격(TCP/UDP 등), hop limit은 IPv4의 ttl값에 해당합니다.

비트오류가 나는 일이 거의 없기에 사실상 의미가 없었던 checksum과, fragment로 쪼개지는 일이 거의 없어 마찬가지로 대부분의 경우에 사용되지 않던 fragment 관련 필드도 빠졌습니다.

### Tunneling



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322164840.png" alt="image-20230322164840490" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230322172606.png" alt="image-20230322172606893" style="zoom:50%;" />



IPv6는 아직 대세로 자리잡지는 못했지만 일부에서는 이를 사용중입니다.

IPv4와의 호환성을 포기하였기에 IPv4만 지원하는 라우터를 통과하기 위해선 Tunneling 작업이 필요합니다.

Tunneling이란 IPv6의 패킷을 payload로 취급하여 IPv4 패킷으로 감싼뒤, IPv6를 지원하는 출발지 쪽 라우터를 출발지로, IPv6를 지원하는 목적지쪽 라우터를 목적지로 삼는 작업입니다.

그림처럼 IPv6를 지원하는 라우터가 IPv4만을 지원하는 라우터로 포워딩을 해야할때 목적지를 IPv6를 지원하는 라우터로 설정하여 Packet을 IPv4의 패킷으로 감싼뒤 전송합니다.

이 패킷이 목적지(IPv6를 지원하는 라우터)에 도착하면 해당 라우터는 패킷을 열어보게되고, 원래의 목적지로 패킷을 보낼 수 있게됩니다.

> 참고로 IPv6를 지원하는 라우터는 현재 30% 정도로 집계되고 있습니다.

