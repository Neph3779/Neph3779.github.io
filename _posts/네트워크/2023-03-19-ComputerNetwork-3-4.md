---
layout: post
title: "[컴퓨터 네트워크] #18 Transport Layer - 4"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Congestion control

flow control이 1대1 상황에서의 overflow 방지를 위한 기법이었다면 congestion control은 네트워크 전체의 delay와 packet loss와 관련된 기법입니다.



### End to end congestion control

End-end 혼잡제어 방식은 두개의 엔드포인트(호스트)간의 혼잡제어 방식입니다.

실제로 congestion 발생지점은 네트워크 내부이지만 호스트가 이를 알아채고 send rate를 조절하는 방식입니다.

네트워크 내부에서 명시적으로 혼잡발생 사실을 알려주지 않는 방법이니 packet loss나 delay를 통해 혼잡 발생 여부를 추측할 수 밖에 없습니다.



### Network-assisted congestion control

host끼리 정보를 주고받던 위의 방식과는 다르게 이번에는 혼잡이 발생한 지점인 라우터가 정보를 제공합니다.

라우터는 3계층 장치이므로 혼잡 발생 사실을 알려줄 수 있는 방법은 ip header를 바꿔주는 것 뿐입니다.

혼잡이 발생하면 해당 데이터의 ip header내의 필드에 혼잡발생 사실을 알리는 비트를 세우고, 이것을 받은 receiver측에서는 이 사실을 transport layer에 알립니다.

이 사실을 인지한 receiver가 ACK등을 통해 sender에게 혼잡발생 사실을 알리면 sender는 이를 통해 send rate를 조절합니다.

### ECN(Explicit Congestion Notification)

ECN은 TCP가 채택하고 있는 Network-assisted congestion control의 구체화된 기법입니다.

ip header의 Tos라는 필드를 활용하며 Tos 라는 2비트 중 하나의 비트를 1로 바꿔서 발송하면 혼잡제어를 이용하겠다는 표시입니다. (00으로 발송하면 혼잡제어를 이용하지 않겠다는 의미입니다.)

라우터는 만약 혼잡 발생시 이 Tos 필드를 11로 바꿔줌으로써 recevier에게 이 사실을 알리고, ACK를 통해 송신자가 발송량을 줄이는 기법입니다.



### AIMD 알고리즘

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230320202249.png" alt="image-20230320202249511" style="zoom: 33%;" />

AIMD는 Additive Increase Multiplicative Decrease의 약자로

네트워크가 혼잡하지 않아 send rate를 증가시킬때는 더하기 단위로 조금씩 증가시키다가

네트워크 혼잡이 발생하면 곱셈 단위로 급격히 send rate를 감소시키는 알고리즘입니다.

> 네트워크 혼잡을 체크하는 트리거는 packet loss입니다.

send rate를 decrease하는 방식 중

TCP Reno 방식에서는 loss 발견 혹은 3rd duplicate ACK 발견시 send rate를 절반으로 낮추며

TCP Tahoe 방식에서는 3rd duplicate ACK는 신경쓰지 않고 loss 발견시 보낼 수 있는 최소 단위의 사이즈인 1MSS(Maximum Segment Size)로 전송비율을 낮춥니다.



### TCP slow start

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230320204055.png" alt="image-20230320204055412" style="zoom: 33%;" />

slow start란 처음에는 보내는 단위를 최소단위인 1MSS로 잡고, 하나의 ACK가 도착할때마다 1MSS만큼 window의 size를 늘리는 기법을 말합니다.

하나의 ACK마다 1MSS씩 늘리게 되므로 segment단위로 도착하는 ACK의 특성상 1RTT당 window size는 2배가 됩니다.

### threshold 활용

threshold는 해당 지점을 넘어가면 언제든 loss가 발생할 수 있다고 판단하여 send rate를 급격히 늘리는 것이 위험하다고 판단하는 지점입니다.

threshold는 timeout이 발생한 window size의 절반값으로 매번 유동적으로 할당합니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230320204112.png" alt="image-20230320204112667" style="zoom: 50%;" />

### TCP CUBIC

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230320210346.png" alt="image-20230320210346636" style="zoom:50%;" />

TCP CUBIC은 timeout이 발생하기까지 send rate를 일정한 비율로 증가시키는 것이 아닌 처음에는 높은 비율로 증가시키다가 일정 지점부터는 완만한 비율로 증가시키는 기법을 말합니다.

마치 3차함수의 모양을 하고 있다고 하여 CUBIC이라는 이름이 붙여졌습니다.



### Delay-Based congestion control

지금까지 살펴본 기법들의 트리거는 "packet loss" 였습니다.

이와달리 dealy-based 혼잡제어 기법은 delay를 통해 congestion 상황을 확인하는 방법으로

측정한 minimum RTT를 기준으로 현재 RTT가 얼마나 커져있는지를 측정하여 delay가 길다고 판단되면 전송비율을 낮추고, 반대라면 전송비율을 높이는 전략을 취합니다.



## Transport Layer의 발전과 QUIC

TCP와 UDP가 너무 예전에 만들어진 프로토콜이라 이를 최적화하거나 개선하는 것만으로는 요구사항을 모두 충족하지 못하는 경우가 많아졌습니다.

이를 해결하기 위해 여러 프로토콜이 등장했고 그 중 가장 대표적인 프로토콜이 바로 QUIC입니다.

QUIC 등장 이전에는 HTTP가 TCP 위에서 돌아가는게 당연하다 생각했었습니다.

TCP가 제공하는 서비스인 신뢰성 있는 전송을 사용해야 했으니까 어찌보면 당연한 생각이었는데,

구글에서 UDP 위에서 HTTP를 돌려보자는게 아이디어를 가지고 제작한 QUIC이라는 게임체인저가 등장합니다.

UDP를 통해 신뢰성과 congestion control을 지원하기 위해 HTTP와 UDP 사이의 중간계층으로 QUIC이 들어가있으며, TCP와 마찬가지로 3-way handshake의 연결설정을 통해 데이터를 주고받게 됩니다.



### QUIC의 연결과정

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230320225014.png" alt="image-20230320225014497" style="zoom:50%;" />

지금까지는 3-way handshake를 다루며 보안에 관련된 이야기를 하지 않았지만 실제로는 왼쪽 그림처럼 TLS handshake가 한 번 더 이루어지게 됩니다.

TLS handshake 과정에선 보안을 위한 보안 기법에 대한 동의, key의 교환 등이 이루어지게 됩니다.

QUIC은 보안까지도 서비스를 제공하므로 이것을 별도의 handshake 없이 하나의 RTT로 통합할 수 있습니다.

변수설정과 이전에 설명한 2-way handshake시의 문제점 등으로 인해 등이 3-way handshake가 필요한 것은 동일합니다.

