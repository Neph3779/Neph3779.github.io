---
layout: post
title: "[컴퓨터 네트워크] #7 App Layer - 1"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

## Application Layer

우리는 sns, web, 문자, email, 게임, 비디오 스트리밍 등 다양한 인터넷 어플리케이션을 사용하고 있습니다.

이러한 인터넷 어플리케이션을 제작하기 위해선 Application Layer에 대한 이해가 필요합니다.



### 어플리케이션 구현시 신경써야할 부분들

**데이터 유실의 허용여부**

Transport 계층에서 받을 서비스가 달라지게 됩니다. (유실이 없어야 한다면 TCP, 아니라면 UDP)

<br/> 

**패러다임**

전통적인 Client - Server 패러다임을 따를것인지 혹은 Peer to Peer 패러다임을 따를 것인지에 대해 결정해야 합니다.

<br/> 

**사용할 프로토콜의 결정**

웹이라면 HTTP를, 이메일 송수신이라면 SMTP & IMAP, host to host 통신을 위한 Socket, 여러  서비스에서 두루 사용되는 DNS 등 다양한 프로토콜을 상황에 맞게 채택하여 사용해야 합니다.



## 인터넷 어플리케이션의 제작

어플리케이션은 대부분 모바일, 랩탑, 데스크탑, 서버 등의 호스트에서 돌아갑니다.

다양한 OS와 환경에 있는 엔드 시스템에서 돌아가게 되므로 네트워크 어플리케이션을 개발한다면 이에 대한 대비가 필요합니다.

다만 인터넷의 계층구조 덕에 어플리케이션 레이어보다 낮은 계층의 내용에 대해서는 고민하지 않아도 됩니다.

가령 라우터에 들어갈 소프트웨어에 대해서는 전혀 고려하지 않아도 되는 것이죠.



### Client-Server 패러다임

**서버**

서버는 항상 인터넷에 연결돼있는 호스트입니다.

DNS를 이용하며 대부분의 경우 고정된 ip 주소를 사용합니다.

수요가 많은 서버라면 데이터 센터를 운영하기도 합니다.

<br/> 

**클라이언트**

클라이언트는 서버와 달리 인터넷에 항상 연결돼있는 것이 아니라 간헐적으로 연결될 수 있음 (서비스를 받을 때만 연결)

다이나믹한 ip주소를 가지고 있어도 문제될 것이 없으며 DHCP 프로토콜을 이용해 ip 주소를 할당받기 때문에 ip주소가 바뀔 가능성이 존재합니다.

Client-Server 패러다임에서 클라이언트는 서버로부터 서비스를 받는 역할이기 때문에 클라이언트끼리는 서로 통신하지 않습니다.

<br/> 

이러한 Client-Server 패러다임을 대표하는 프로토콜로는 `HTTP`, `IMAP`, `FTP`(파일 전송) 등이 있습니다.



### Peer-Peer 패러다임

P2P 패러다임에서는 항상 인터넷에 연결되어 있는 서버가 존재하지 않습니다.

더 정확하게는 서버라는 용어도 사용하지 않고 `peer`라는 용어를 사용하며 

peer는 서버의 역할과 클라이언트의 역할을 동시에 합니다.

서버의 역할을 하는 peer일지라도 ip도 자주 바뀌고 항상연결돼있지 않을 수 있기에 관리가 복잡하다는 특징을 가지고 있습니다.



### Process communicating

같은 컴퓨터 내의 프로세스가 서로 통신해야 한다면 IPC(Inter Process Communication) 메커니즘에 의해 통신이 가능하며, 이는 네트워크가 아닌 OS의 영역입니다.

하지만 서로 다른 호스트에서 돌아가는 프로세스들 사이의 통신이 필요하다면 네트워크의 도움을 받아야합니다.

이 경우 프로세스는 `클라이언트 프로세스`와 `서버 프로세스` 두 가지 종류로 구분됩니다.

`클라이언트 프로세스`는 서비스를 **요청**하는 쪽에 해당하며 **통신을 시작**하는 역할을 하고

`서버 프로세스`는 서비스를 제공하는 쪽이며 **통신을 기다리다가** 요청이 들어오면 **서비스를 제공**합니다.

P2P 패러다임에서는 각각의 `peer`가 클라이언트와 서버의 역할을 모두 하므로, 호스트는 클라이언트 역할 프로세스와 서버 역할 프로세스를 모두 가지고 있습니다.

 

### Sockets

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230218212953.png" alt="image-20230218212953246" style="zoom:50%;" />

소켓이란 용어는 유닉스 계열 운영체제 중 하나인 `BSD`에서 처음 등장했습니다.

소켓은 네트워크 프로그래밍을 위한 인터페이스로, 호스트간의 통신에서 사용되는 API입니다.

집 앞에 우편물을 놔두면 배달부가 목적지에 알아서 잘 전달해주는 것처럼

보낼 메세지를 소켓에 맡기면 이후에 일어나는 일련의 과정에 대해 신경쓰지 않아도 목적지에 메세지가 도착하게 되므로 개발자 입장에선 편하게 작업할 수 있습니다.

<br/>BSD에서 처음 이 인터페이스를 제공할땐 C언어로만 제공했지만, 현재는 다양한 언어에서 지원되며, 현존하는 대부분의 OS에서 소켓 인터페이스를 제공하고 있습니다.



### **IP주소와 포트번호**

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230218215130.png" alt="image-20230218215130160" style="zoom:50%;" />

ip주소는 호스트의 네트워크 인터페이스에 부여되며 32bit로 표현됩니다.

64bit를 사용하는 IPv6가 개발된지 오래지만 지금 사용되지 못하고 32bit IPv4를 사용하고 있습니다.

> IPv6가 사용되고 있지 않은 이유에 대해서는 추후에 설명할 예정입니다.

<br/>

프로세스 간의 통신을 위해서 각각의 컴퓨터를 지정하는 작업이 필요한데 이때 사용되는 것이 ip주소입니다.

하지만 ip주소는 컴퓨터의 네트워크 인터페이스에 부여되는 것이지 특정 프로세스까지 지정할 수 없으므로

프로세스를 지정하기 위해 등장한 것이 바로 포트번호 입니다.

<br/>

포트번호는 컴퓨터 내의 프로세스를 특정하는 역할을 하므로

다른 호스트의 특정 프로세스에 데이터를 보내기 위해선 목적지 컴퓨터의 ip주소와 프로세스의 포트번호가 모두 있어야지만 데이터를 전달할 수 있습니다.

대표적인 포트번호로는 80번의 http server와 25번을 사용하는 email server가 있습니다.

> 본 글에 사용된 자료는 [https://gaia.cs.umass.edu/kurose_ross/ppt.php](https://gaia.cs.umass.edu/kurose_ross/ppt.php)에서 무료로 제공하고 있습니다.
