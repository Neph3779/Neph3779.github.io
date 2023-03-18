---
layout: post
title: "[컴퓨터 네트워크] #16 Transport Layer - 2"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## UDP

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230316154649.png" alt="image-20230316154649822" style="zoom:50%;" />

UDP는 약간의 데이터 로스, 순서 뒤바뀜을 감당할 수 있는 서비스들이 사용하는 4계층 프로토콜입니다.

HTTP/3는 예외적으로 QUIC과 함께 사용해서 신뢰성과 순서를 보장합니다. (congestion control도 지원)

<br/> **UDP sender**는

UDP datagram의 header field를 정의하고 (소켓이라면 mux과정 포함)

application message를 udp datagram으로 바꾼 뒤

각각의 datagram을 아래 계층에 내려보냅니다.

<br/>

**UDP receiver**는

아래 계층으로부터 datagram을 수신받고

checksum 헤더를 체크해서 이상이 없는지 확인한 뒤

어플리케이션 레이어에 datagram을 메시지로 만들어서 올려보냄

소켓이라면 demux를 통해 어떤 소켓을 올려보낼지도 결정하게 됩니다.



### checksum

checksum은 비트 플리핑(오류) 발생시에 문제가 발생했다는 사실을 알릴 수 있는 헤더 필드입니다.

sender는 데이터를 16비트 단위로 잘라서 모두 더하는 방식으로 checksum을 제작합니다.

receiver는 이 필드를 확인하여 데이터가 훼손되었는지를 확인합니다.



## TCP

TCP의 가장 큰 특징은 신뢰성 있는 데이터 전송입니다.

어플리케이션 레이어 입장에서는 transport가 알아서 reliable한 데이터 전송을 책임질 것이라 생각하고 어떻게 구현하는지는 관심을 가지지 않고

네트워크 레이어에서는 reliable한 전송을 지원하지 않으니 transport 계층에서 이를 지원해야 합니다.

TCP를 이용하는 sender와 receiver는 서로 각각의 상태를 알지 못합니다. (stateless)

현재 시점에 내가 나의 상태를 상대에게 보내도  상대가 그 상태정보를 수신하는 시점에는 내 상태가 변해있을 수 있기 때문입니다.

### rdt3.0 채널

TCP는 rdt3.0 채널을 이용합니다.

rdt3.0 채널은 에러와, 데이터 유실이 있을 수 있는 채널이며 data와 ACK를 전송할 수 있습니다.

ACK는 sender가 데이터 보냈을때 receiver가 데이터를 잘 받았다는 의미로 보내는 값입니다.

rdt3.0 채널에서는 데이터 유실이 발생할 수 있으므로 ACK도 유실될 수 있습니다.

sender는 ACK를 받지 못하면 데이터를 다시 보내게 되는데 이때 기다리는 시간은 1RTT에 추가적인 알파값을 더한 시간만큼 기다리게 됩니다. 너무 오래 기다려도, 너무 짧게 기다려도 성능에 안좋은 영향이 있으므로 적당한 값을 알파에 넣어야 합니다.

만약 일정시간동안 ACK가 오지 않아 데이터를 다시 보냈는데, 공교롭게도 ACK를 보내자마자 데이터를 받은 경우가 존재할 수 있는데, 이때 receiver는 중복된 패킷을 전달받게 되므로 이를 구분하기 위한 seq#가 필요합니다.

seq#이 3이라면 패킷에 0, 1, 2, 3을 차례대로 순회하며 보내게 되며, 수신자는 이 값을 통해 데이터 중복 여부를 확인합니다.



### rdt3.0 sender state transition diagram

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230317160722.png" alt="image-20230317160722660" style="zoom:33%;" />

상태천이도를 통해 rdt3.0 채널을 이용한 데이터 전송에 대해 살펴보면

1. 0번 데이터를 보내라는 명령이 들어옵니다.
2. packet을 만들어서 udt_send를 통해 발송합니다.
3. time out을 체크하기 위한 타이머를 가동시킵니다.
4. ACK0를 기다립니다.
5. ACK0를 받았다면 checksum 확인해서 오염되지 않았다면 타이머를 종료합니다.

<br/>

만약 에러나 예상 이상의 delay가 발생하지 않았다면 위와 같은 시나리오를 반복하게 됩니다.

만약 delay가 발생해 timeout이 일어났다면 그 위치에서 다시 send후 타이머를 재가동시키며

데이터가 오염되었거나 이미 ACK값을 수신받았을 경우에는 NOP 액션 즉, 아무것도 하지 않습니다.



### rdt3.0 Pipeline

만약 하나의 데이터를 보내고 해당 요청에 대한 ACK를 받을때까지 기능을 정지한다면 성능상으로 매우 안좋을 것입니다.

이를 해결하기 위해 pipelining을 통해 ACK가 도착하기 전에도 다음 데이터를 전송하는 기법을 사용합니다.



## Go-Back-N, Selective repeat

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230317175022.png" alt="image-20230317175022808" style="zoom:50%;" />

Go-Back-N 기법과 Selective repeat 기법은 보낸 데이터, 혹은 아직 보내지 않은 데이터를 보관할지 말지에 대해 결정할때 사용하는 아이디어입니다.

receiver로부터 ACK가 오지 않은 경우에는 데이터를 무조건적으로 다시 보내게 되는데 receiver가 보낸 ACK가 유실된 것이라 receiver 쪽에서는 데이터를 다시 받을 필요가 없는 상황이더라도 sender는 이를 알 수 없으므로 데이터를 재전송합니다.

window는 아직 ack가 도착하지 않은 데이터들과 아직 보내지 않은 데이터들을 가지고 있으며 receiver가 연속된 패킷을 받았다면, 패킷의 가장 마지막 번호에 대한 ACK 전송하게 되며, sender는 해당 번호까지는 모두 ACK가 도착한 것으로 간주합니다.

Go-Back-N 프로토콜에서는 n번째 데이터를 받지 못한 상황에서 n+1, n+2번째 데이터가 들어온다면 n+1번째, n+2번째 데이터에 대한 ack값을 n으로 보내게 되며, sender는 이 ack값을 시작으로 다시 데이터를 하나씩 보내게 됩니다.

(n+1, n+2번째 데이터는 버립니다.)

이렇다보니 Go-Back-N 프로토콜에서는 time out을 측정할 timer가 하나만 필요합니다. 

이와 다르게 Selective repeat은 window 내의 각각의 데이터에 대해 timer를 두는 방식입니다.

만약 n번째 데이터를 받지 못한 상태에서 n+1, n+2번째 데이터를 받았고, window 내의 범위에 속한 데이터라면 이를 유지합니다.

sender는 ack를 받지 못한 n에 대해 다시 데이터를 보내게되며, receiver는 이 빠진 데이터(n)가 채워지면 window를 slide할 수 있게 됩니다.

selective repeat방식에서는 엉뚱한 seq#가 채워지는 것을 방지하기 위해 sequence#의 최댓값은 window size의 2배보다 커야합니다. (다음 cycle의 seq#를 보내게 될 수 있으며, 이때 이전 cycle의 seq#가 채워질 수 있기 때문)

TCP는 Go-Back-N 기법과 Selective repeat 기법의 일부를 채택하여 사용하고 있습니다.