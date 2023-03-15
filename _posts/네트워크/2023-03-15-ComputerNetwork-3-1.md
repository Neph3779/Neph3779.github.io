---
layout: post
title: "[컴퓨터 네트워크] #15 Transport Layer - 1"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Transport Layer의 서비스

`Network layer`에서는 `host to host` 서비스를 제공하며

`Transport layer`에서는 이보다 구체적인 단계인 `process to process` 서비스를 제공합니다.

송신측에서는 어플리케이션 레이어로부터 전달받은 message를 TCP segment로 나누어 네트워크 레이어로 전달하고,

수신측에서는 나눠진 segment들을 하나의 메시지로 다시 합쳐 어플리케이션 레이어로 올려보내는 작업을 진행하게 됩니다.



## Multiplexing, Demultiplexing

`MUX`라고 줄여서 부르는 Multiplexing은 한국어로 번역하면 다중화입니다.

다중화란 `demux`때 올바른 소켓에 데이터를 전달하기 위해 필요한 정보를 transport 헤더에 포함시키는 작업입니다.

다중화와 역다중화는 `connectionless(UDP)` / `connection(TCP)` 방법 두가지가 존재합니다.



### UDP(connectionless)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230315180553.png" alt="image-20230315180553499" style="zoom: 33%;" />

UDP는 연결설정이 필요하지 않으므로 수신측의 ip 주소와, 포트번호만 필요합니다.

UDP를 통한 소켓 통신에서 서버는 하나의 소켓을 통해 여러개의 클라이언트의 요청을 수용할 수 있습니다. (다수의 요청을 Parallel하게 처리할 수 있다는 것과는 다릅니다.)



### TCP(connection)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230315180754.png" alt="image-20230315180754845" style="zoom:33%;" />

UDP와 다르게 TCP는 처음에 연결작업이 필요합니다.

이 연결작업은 1:1 매칭이기 때문에 하나의 포트를 공통창구처럼 사용할 수 없습니다.

요청이 들어오면 해당 source 전용의 새로운 소켓을 만들어 연결을 진행합니다.

> 이때 소켓들은 source의 ip주소와 port번호로 구분됩니다.
>
> 하나의 호스트가 여러개의 요청을 보낼수도 있으므로 ip주소만 가지고 구분할 수 없고 포트번호가 반드시 필요합니다.
>
> 추가로 P4 P5 P6의 ip주소와 포트번호는 모두 같습니다.

<br/> 

정리하자면 Mux는 누구에게 보내는 것인지를 표시하는 과정,

Demux는 헤더를 보고 구분해서 적절한 소켓으로 전달하는 과정이라 할 수 있습니다.



## UDP: UserDatagram Protocol

UDP는 4계층 프로토콜이지만 있지만 3계층에서 제공한 서비스에 추가적으로 무언가를 제공하지는 않습니다.

ip 프로토콜이 제공하는 서비스가 host to host의 데이터 전달이라면 UDP는 한단계 더 구분해서 해당 데이터를 process로 전달하는 것이 역할의 전부입니다.

UDP는 데이터 유실이 발생할 수 있으며 (Datagram/Packet loss) 순서가 뒤바뀔 수 있습니다.

이미 언급했다시피 필연적으로 유실과 순서 뒤바꿈이 발생하는 것은 아니며, best-effort 서비스일 뿐입니다.

Datagram은 홀로 취급되는 전송단위라는 뜻으로, 단독으로 처리되는 데이터를 말합니다. (더 이상의 분해 없이)

<br/>

UDP는 TCP에 비해 제공하는 기능만 놓고보면 하위호환이라 볼 수 있지만

TCP connection이라는 과정에서 발생하는 오버헤드를 가지지 않습니다. (시간적 오버헤드, 공간적 오버헤드)

TCP에서 제공하는 혼잡제어도 필요없기 때문에 같은 네트워크 상태가 혼잡하더라도 UDP 요청을 여러개 보내서 운 좋게 하나의 응답을 받아내는 꼼수도 사용할 수 있습니다. 

> TCP는 네트워크 과부하를 막기 위해 보내는 데이터 양을 제한하거나 속도를 제한하기도 하는데, UDP는 그런 제한이 없습니다.

UDP를 사용하는 대표적인 서비스에는 `스트리밍 서비스`, `DNS`, `HTTP/3` 등이 있습니다.