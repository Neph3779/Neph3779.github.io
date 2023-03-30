---
layout: post
title: "[컴퓨터 네트워크] #17 Transport Layer - 3"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## TCP의 특징

### cumulative ACK

TCP는 cumulative ACK를 사용합니다. cumulative ACK란 누적되는 ACK라는 뜻으로 연속된 ACK만을 누적시켜 현재 최종 ACK를 표기하는 방법입니다.

### pipelining

TCP는 window size와 flow control이 허용하는 범위 내에서 pipelining을 통해 통신을 진행합니다.

이는 in-flight segment가 1보다 크다는 의미와도 같습니다.

### flow controlled

흐름제어란 sender가 receiver의 window size를 고려하여 데이터를 보냄으로써 overflow를 방지하는 기법입니다. 현재 수용할 수 없는 데이터는 보내봐야 자원의 낭비이므로 보내지 않는다는 의미와도 같습니다.



## TCP segment의 구조

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230319104313.png" alt="image-20230319104313545" style="zoom:50%;" />

UDP 헤더가 8Byte에 불과했던 것에 반해 TCP의 헤더는 최소 20Byte의 길이를 가집니다. (option이 추가되면 더 늘어날 수 있지만 거의 사용되지 않습니다.)

data 부분은 byte단위로 일련번호가 붙게되며, ACK번호는 "수신자의 입장에서 다음번에 받기를 기대하는" 데이터의 일련번호가 기재됩니다.

만약 수신자가 받은 data에 100번부터 199번까지의 데이터가 포함되어 있었다면 ack에는 200번이 실려 보내지게 됩니다.

헤더에 속해있는 CEUAPRSF의 필드는 각각 1비트의 필드로 CE 비트는 혼잡제어, RSF 비트는 TCP connection의 강제종료, 연결설정, 정상종료를 알리기 위해 사용됩니다.

receive window는 flow control을 위해 사용됩니다. 송신자가 수신자의 window size를 고려하여 데이터를 보낼 수 있도록 현재 window의 capacity를 알려줍니다.



### TCP seq#의 구조

TCP는 cumulative ACK를 사용하니 모든 segment마다 ACK를 보낼 필요는 없습니다.

순차적으로 누적된 ACK의 마지막 번호만을 보내면 송신자 입장에서는 해당 번호까지의 데이터가 잘 전송되었으리라 생각할 수 있습니다.

순서 뒤바뀜을 해결하기 위해서는 out-of-order 데이터를 보관하는 방식을 채택합니다. window 범위 내의 out-of-order 데이터에 한해 이를 보관하고 있다가 빠진 데이터가 채워지면 이를 반영한 ACK를 보내게 됩니다.

> RFC 문서에는 순서 보장을 구현하는 방법에 대한 규약이 없으며, 구현자가 알아서 구현할 것을 요구하고 있으므로 구현 방법은 달라질 수 있으나 모든 TCP가 보관방식을 채택하고 있습니다.



## timeout

timeout은 재전송을 하기 전, ACK가 도착하기까지 기다리는 시간을 의미합니다. timeout 시간이 너무 짧으면 재전송을 지나치게 많이 하게되고, 너무 길면 성능의 낭비가 생기니 적절하게 정하는 것이 중요합니다. 

timeout은 1RTT + 알파의 값으로 정하게 되는데, RTT와 알파 모두 동적으로 할당해주어야 변화하는 상황에 맞게 timeout을 설정할 수 있습니다.

RTT를 측정하기 위해 빈 데이터를 실은 segment를 보내 SampleRTT를 구한 뒤, 이번에 측정한 SampleRTT 데이터와 이전에 측정했던 누적 SampleRTT 데이터를 조합한 값을 timeout으로 지정하게 됩니다.

이번에 측정한 SampleRTT의 값이 일시적인 네트워크 혼잡에 의해 지나치게 길거나, 운이 좋게 빨리 도착해 매우 짧을 수 있으므로 기존의 RTT들을 활용해 가중치를 두는 방식입니다.

가령 x%의 가중치를 둔다고 하면, 기존의 RTT는 x%, 이번에 측정한 RTT는 (1-x)%를 반영합니다.

알파값은 safety margin이라고 부르는데, 이 safety margin은 SampleRTT의 변화폭이 크다면 네트워크의 상태가 크게 변할 수 있다고 판단해 큰 값을 할당하며, 그 반대라면 안정적인 상태이므로 작은 값을 할당합니다.

TCP에서 timeout을 측정하는 타이머는 ACK를 받지 못한 데이터 중 가장 오래된 데이터에 대한 타이머 하나만을 가동합니다.



### 실제 TCP의 상황별 동작

| Event at receiver                                            | TCP receiver action                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 제대로된 순서의 segment가 도착하였고, 이전에 도착한 seq#들이 모두 ACK된 경우 | 이번에 도착한 데이터에 대한 ACK 전송을 0.5초간 하지 않다가, 0.5초가 지나도록 새로운 데이터가 도착하지 않으면 ACK를 전송(모든 데이터에 대한 ACK를 보내는 것이 낭비이므로 ) |
| 제대로된 순서의 segment가 도착했지만, 이전 segment에 대한 ACK를 아직 보내지 않은 경우 (0.5초간의 기다림 동안 새로운 segment가 도착한 경우) | 이번에 도착한 segment에 대한 ACK를 발송                      |
| 순서에 어긋난 segment가 도착한 경우                          | 순차적으로 누적된 segment중 가장 큰 값에 대한 ACK를 발송 (sender 입장에서 신속하게 빠진 데이터를 알게 하기 위함) |
| 비어있던 공간을 채워주는 segment가 도착한 경우               | 순차적으로 누적된 segment중 가장 큰 값에 대한 ACK를 발송     |



### Fast retransmit

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230319164019.png" alt="image-20230319164019653" style="zoom:50%;" />

재전송은 timeout이 발생해야 진행하는것이 원칙이지만, 위의 그림처럼 loss가 "거의" 확실한 상황에서는 timeout을 무시하고 바로 재전송을 하기도 합니다. 빠른 재전송의 조건은 3개 이상의 중복된 ACK를 받는 것입니다. 

## TCP flow control

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230319194630.png" alt="image-20230319194630014" style="zoom:33%;" />

app이 데이터를 읽어가면 tcp window를 slide하게 됩니다. 이때 남아있는 버퍼의 크기를 receive window 필드에 실어보내게 되며 sender는 이 receive window를 통해 overflow를 방지합니다.

receive window의 크기는 비어있는 버퍼의 크기를 나타내지만 중간에 비어있는 segment (아직 도착하지 않은 segment)의 공간을 고려하지는 않습니다. 다시 말해 가장 마지막 순서의 segment부터 window의 끝까지의 공간 중 비어있는 공간의 크기를 실어 보내게 됩니다.

## TCP connection 관리

TCP는 연결 설정, 데이터 교환, 연결 해제의 3단계를 거치게 됩니다.

3-way handshake 과정을 통해 1대1 연결을 진행하며, 환경변수 등의 세팅이 이루어지게 됩니다.

이때 seq#의 초기값을 공유하게 되며, 이는 isn(initial sequence number)라고 부릅니다.

sender와 receiver는 서로가 데이터를 주고 받으므로 각자가 send, receive buffer를 가지고 있어야 합니다.



### 3-way 여야 하는 이유

단순하게 생각해보면 연결요청 - 승인 - 데이터 전송의 3과정이 아닌 연결요청과 함께 데이터 전송 - 승인과 함께 데이터 수신의 2과정으로도 해결할 수 있을 것 같은 TCP 연결설정은 다음과 같은 시나리오들로 인해 3-way여야 합니다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230319195709.png" alt="image-20230319195709506" style="zoom:33%;" />

위의 시나리오처럼 클라이언트가 연결요청을 했는데 서버로부터의 응답이 늦어 timeout이 발생하였고, 공교롭게도 재연결 요청이 TCP 연결 종료 이후에 도착하는 경우가 있을 수 있습니다.

클라이언트는 연결할 의사가 없음에도 서버는 연결을 열어놓고 기다리게 되므로 자원낭비가 발생합니다.



### TCP 연결 종료

TCP의 연결 종료는 FIN비트를 세팅해서 전송함으로써 연결종료를 알릴 수 있습니다. 이 FIN에 대한 ACK를 수신하면 비로소 TCP 연결이 종료됩니다. 

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
