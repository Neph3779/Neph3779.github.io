---
layout: post
title: "[컴퓨터 네트워크] #10 App Layer - 4"
image:
categories: [네트워크]
tags: 
  - 컴퓨터네트워크
  - Computer Science
sitemap:
  changefreq: daily
  priority: 1.0
---

이번 글은 HTTP Request, Response에 대해 다룹니다.

[HTTP 기본 개념 정리 with swift](https://neph3779.github.io/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/HTTPBasicWithSwift/)에서도 다룬 내용이니 중복되는 부분은 간략히 작성하고 넘어가겠습니다.

## HTTP Message Type

HTTP는 `Request`와 `Response` 총 두가지 종류의 메시지가 있습니다.

이 메시지들은 ACSII format이기 때문에 읽는데 큰 수고가 들지 않습니다.

### HTTP Request

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230301194224.png" alt="image-20230301194224283" style="zoom:50%;" />

리퀘스트 메시지의 생김새는 위와 같습니다.

첫 줄에는 HTTP 메서드, 요청하는 URL, HTTP 버전을 실어보내며

두번째 줄부터는 HTTP header들을 각줄마다 하나씩 작성하게 됩니다.

그 이후 `CRLF`를 한번 더 입력하여 body의 시작을 알린뒤 body가 실리게 됩니다.

> CRLF: Carriage Return, Line Feed의 약자로, 줄을 바꾸고 커서를 가장 앞으로 보내는 명령어입니다. 쉽게 말해 Enter키를 한번 입력하는 것과 같습니다.



## HTTP Methods

HTTP 메서드는 GET, POST, PUT, PATCH 등 다양하게 존재합니다.

GET 메서드는 qeury string이나 path variable을 통해 특정 데이터를 받아오는 역할을,

POST는 어떠한 정보를 서버에 body에 실어 보내는 역할,

PUT과 PATCH는 서버 내의 정보를 수정, 덮어쓰기 등의 역할을 하게 됩니다.

PUT은 실어서 보내지 않은 내용을 전부 디폴트 값으로 바꾸므로, 객체의 내용을 전부 수정해야할때 주로 사용되며

PATCH는 실어보낸 특정 부분만을 수정하므로 객체의 내용을 일부만 수정할 때 주로 사용됩니다.



### 멱등성

>  멱등성: A라는 행동을 여러번 했을때, A라는 행동을 한번 했을때와 결과가 같은가?

POST의 경우 실행할때마다 서버에 URI가 추가되므로 멱등하지 않지만

PUT의 경우 아무리 반복해도 서버는 처음 PUT 요청을 받았을때와 동일한 상태이므로 멱등합니다.

멱등하기 때문에 reponse를 캐싱할 필요가 없는 것이죠. 

더 자세한 내용은 [Put vs Post](https://restfulapi.net/rest-put-vs-post/)를 참고해주세요



## Cookies

인터넷을 쓰다보면 빠지지 않고 등장하는 용어 중 하나가 바로 "쿠키"입니다.

쿠키는 stateless protocol이라는 HTTP의 한계를 극복하기 위해 만들어졌습니다.

서버에 전달되는 모든 HTTP Request는 독립적이므로, 어떤 유저가 브라우저를 통해 장바구니에 물건을 담더라도

유저를 식별할 수 없기 때문에 서버는 이 사실을 알 수 없습니다. (회원가입 등을 통한 유저식별은 없다고 가정하겠습니다.)

이러한 상황을 해결하기 위해 등장한 것이 쿠키입니다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230301205717.png" alt="image-20230301205717914" style="zoom:50%;" />



쿠키는 웹사이트와 브라우저 사이에 state가 유지되도록 도와주는 특별한 식별자입니다.

클라이언트가 처음으로 서버에 request를 보내면 서버는 response에 cookie값을 실어서 보내주고 그 값을 자신의 백엔드 데이터베이스에 저장합니다. (set-cookie)

브라우저는 cookie를 저장해두었다가 해당 웹사이트에 request를 보낼때마다 cookie값을 같이 실어서 보내게 됩니다.

이를 통해 서버는 유저의 정보를 식별하고 관리할 수 있습니다.

클라이언트에서 쿠키를 지웠을때 서버는 이를 알 수 있는 방법이 없습니다.

 

### 쿠키의 사용처

쿠키는 여러 방면에서 사용됩니다. 권장되지는 않는 방법이지만 유저를 인증하는데도 사용될 수 있고, 앞서 언급한 예제처럼 유저의 쇼핑카트를 유지하기 위해서도 사용될 수 있죠.

이런 쿠키는 `광고`에도 사용될 수 있습니다. 가령 A 사이트가 광고회사와 계약을 맺었다면 

A 사이트에 접근하는 유저로 하여금 A 사이트뿐만 아니라 X라는 광고 사이트에 접근하도록 하여 (팝업 없이도 접근시키는것은 얼마든지 가능합니다.) X 사이트의 쿠키도 보관하도록 할 수 있습니다.

광고 사이트 X가 여러 사이트에 계약을 맺었다면 사용자가 다른 사이트에 접근했을때에도 그 사실을 알 수 있으며

A사이트에 접근해서 전달된 쿠키인지, 또는 B사이트에 접근해서 전달된 쿠키인지 등을 식별하여 더 다양한 정보를 얻어낼 수도 있습니다. 

> X사이트로부터 전달되는 쿠키는 모두 완전랜덤한 것이 아닌, 서버에서 식별할 수 있는 구분자 등을 추가하여 보낼 수 있습니다. 이러한 방법으로 cookie를 tracking하여 관련정보를 더 자세히 얻어낼 수 있습니다.
