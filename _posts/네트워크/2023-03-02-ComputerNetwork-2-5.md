---
layout: post
title: "[컴퓨터 네트워크] #11 App Layer - 5"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Web caches

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230303211700.png" alt="image-20230303211700747" style="zoom:50%;" />

웹 캐시는 Proxy Server라고도 불리며, 진짜 서버에 접근하지 않고 response를 주기 위한 목적으로 존재합니다.

유저의 입장에서는 response time이 줄어드는 이점이 있고, 기관이나 기업 입장에서도 ISP로 나가는 traffic을 줄여 망사용료 측면에서도 이득을 볼 수 있습니다.

> ISP에 더 비싼 요금제를 지불하는 것을 통한 해결이 웹 캐시보다 이득인 경우도 있으므로, 계산을 통해 손익을 따져보고 적용해야 합니다.

웹 캐시는 클라이언트 입장에서 보았을땐 서버의 역할을 하며, 진짜 서버의 입장에서 보았을때는 클라이언트의 역할을 합니다.

클라이언트로부터 요청이 들어왔을때 캐싱해놓은 response가 유효하다면 (유효함의 기준은 정책마다 달라질 수 있습니다.) 해당 response를 전달하며, 유효하지 않다면 서버로부터 response를 받아와 그것을 클라이언트에 전달합니다.



### Cache-control

진짜 서버는 response를 내려줄때 Cache-control이란 필드에 캐시 정책에 대한 내용을 실어 보내주기도 합니다.

`max-age=<seconds>`는 seconds동안에는 캐시가 유효함을 보장한다는 의미이며,

`no-cache`는 캐시를 저장하되, 유효성 확인이 매번 필요하다는 의미,

`no-storage`는 캐싱을 해도 소용이 없으니(매번 response가 변하니) 캐싱을 하지 말라는 의미를 지닙니다.

### CDN

CDN은 구글, 아마존, 넷플릭스같은 거대 기업에서 이용하는 지역 서비스 네트워크입니다. ([Introduction - 4](https://neph3779.github.io/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/ComputerNetwork-1-4/)를 참고해주세요)

웹 캐시를 통해 특정 지역에서의 traffic을 해결할 수도 있으며, 이를 잘 활용하여 경제적 이득을 가져올 수도 있습니다.



### Conditional GET

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230303211832.png" alt="image-20230303211832112" style="zoom: 33%;" />

웹 캐시 입장에서 캐싱한 내용이 최신인지 어떻게 알 수 있을까요?

답은 `Conditional GET`에 있습니다.

서버가 response를 보내줄때 `last-modified` 필드를 같이 실어보내주게 되는데

이 last-modified를 보관하고 있다가, 캐시가 유효한지 확인하는 `Conditional GET` request를 보낼때 실어서 보내주게되면 

서버는 이것이 유효한지(변경되지 않았는지)를 확인하여 유효하다면 304번 상태코드와 함께 헤더필드만을, 유효하지 않다면 200번 상태코드와 함께 변경된 데이터를 실어서 보내줄 수 있습니다.

response 메시지를 보내는건 똑같지만 캐시가 유효한 경우 데이터를 실어보내지 않아도 되니 성능상에 큰 이점을 가져옵니다.



## HTTP/2

HTTP2는 HTTP 1.1의 한계를 극복하기 위해 만들어졌습니다.

HTTP2의 가장 큰 목표는 multi object HTTP requests의 delay를 줄이는 것입니다.

HTTP 1.0 때는 non-persistent를 채택하고 있었기 때문에 tcp 커넥션 하나당 하나의 데이터를 주고받을 수 있었지만 

HTTP 1.1은 persistent였기에 하나의 tcp 커넥션에서 여러개의 get 보내는게 가능해졌습니다.

하지만 여전히 존재하는 문제가 있었고 그것이 바로 HOL Blocking 문제입니다.

### HOL Blocking

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230303211928.png" alt="image-20230303211928498" style="zoom: 33%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230303211948.png" alt="image-20230303211948125" style="zoom:33%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                 HTTP 1.1 (전송단위: Object)                  |                  HTTP 2.0 (전송단위: Frame)                  |



HOL Blocking 문제란 하나의 거대한 오브젝트를 요청한 상황에서 서버의 입장에서는 FCFS로 서비스를 제공할 수밖에 없으니 

이후에 발생한 요청들이 모두 기다리고 있어야 하는 문제입니다.



이를 극복하기 위해 HTTP2는 다음과 같은 해결책을 내놓았습니다.

1. 요구하는 자원의 priority를 지정
2. Push 테크닉 적용 
3. 전송단위의 변경 (`Object` to `Frame`)

요구하는 자원의 중요도를 클라이언트가 결정해서 알려줌으로써 더 시급한 자원을 일찍 받아볼 수 있게 되었습니다.

Push 테크닉이란, 서버가 클라이언트의 요청을 예측해서 미리 response를 전달해주는 것으로, 클라이언트가 요청하기 전에 서버가 클라이언트의 패턴을 예측해 미리 response를 전달해주는 기법을 말합니다.

가장 중요한 변경사항인 전송단위의 변경은 기존에는 `Object`단위로 데이터를 전송했었지만 이 `Object`를 `Frame`으로 쪼개어서 전달하자는 아이디어입니다.

그림상의 O1의 요청이 먼저 들어왔음에도 Frame 단위로 전송하니 O2, O3, O4가 먼저 도착할 수 있게 되어 HOL Blocking을 완화시킬 수 있습니다.

### HTTP3

HTTP2에도 여전히 한계는 존재합니다.

HTTP2는 하나의 tcp connection위에서의 동작합니다.

TCP를 이용하고 있다면 packet loss를 복구하기 위해 이후의 전송들이 기다리는 문제는 필연적으로 발생합니다.

이를 해결하기 위한 아이디어로 클라이언트가 **여러개의 tcp connection**을 열어 데이터 각각을 각각의 tcp 연결에서 요청하는 꼼수가 등장합니다.

서버 입장에선 여러개의 tcp connection을 연결해줘야 하니 부담이 되겠지만 클라이언트 입장에선 최적의 성능을 구사할 수 있죠.

상황이 이렇게 변하자 또 다른 아이디어가 등장하는데 **TCP를 포기하고 UDP를 사용하자**는 것이 그 발상입니다.

UDP를 쓰자는 것은 TCP에서 보장하는 ordering, packet loss 복구를 모두 포기하자는 것인데 이게 어떻게 가능할까 싶지만

구글에서 제안한 QUIC이라는 HTTP3 바로 아래에서 동작하는 프로토콜을 통해 해결이 가능합니다.

쉽게 말하자면 L4 계층에서 서비스하던 에러 컨트롤(packet loss 복구, ordering)을 L5 계층에서 맡아서 진행하겠다는 아이디어입니다.

QUIC은 이러한 에러 컨트롤을 담당하며 application layer 입장에서는 마치 TCP를 쓰듯이 UDP를 사용할 수 있게 되었습니다.

HTTP3는 아직 RFC 문서로 등록되어있지는 않지만 유명 기업들에서 HTTP3를 밀고 있기 때문에(구글) 등록이 유력한 상황입니다.