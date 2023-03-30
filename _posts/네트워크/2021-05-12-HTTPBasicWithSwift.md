---
layout: post
title: "HTTP 기본 개념 정리 with swift"
image:
categories: 네트워크
tags: 
  - HTTP
  - multipart/form-data
sitemap:
  changefreq: daily
  priority: 1.0
---

## HTTP란?

CSS, HTML, 이미지 등이 `Server와` `Client가` 서로 주고받는 Content라면

Content를 주고받기 위해 서로가 알아들을 수 있는 공통의 약속(`Protocol`)이 필요하다.

이 약속이 바로 `HTTP`(Hyper Text Tranfer Protocol)이다.

## Request와 Response

HTTP는 크게 Request와 Response로 구분된다.

Client가 Request 메세지를 작성해서 Server에 보내면 Server는 Response를 보내준다.

Request와 Response Message는 각각 Header와 Body로 구분된다.

## HTTP Message 구조

Request의 Message는 다음과 같은 구조로 이루어져 있다. 

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210511230558.png)

Header와 Body로 구분되며 그 사이에는 반드시 blank line이 들어가야한다.



### Header의 구조

가장 첫 줄은 Request Line으로 `HTTP Method Name` + `요청할 파일의 형식/요청할 파일의 이름` + `사용할 HTTP 버전`으로 이루어져있다. 

Request Line을 제외한 나머지 부분을 Request Header라고 부르는데 필수로 적어야하는 Host Header를 제외하면 다른 Header는 꼭 있어야 하는건 아니다. 

이번 프로젝트에서 중요하게 이용되는 HTTP Header인  Conten-type에는  Body에 들어갈 Message의 MIME(Multipurpose Internet Mail Extensions) type을 적어줄 수 있다.



### Body의 구조 

MIME type은 여러가지 종류가 있는데 가장 대표적인 것은 HTML 코드이며,  이번 프로젝트에서는 `multipart/form-data`과 `JSON`을 사용한다. JSON에 비해 상대적으로 익숙하지 않은 multipart/form-data를 아래에 정리해보았다. 

- MIME Type에 대해 설명하고 있는 mozilla 문서: [MIME 타입](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
- 더 많은 MIME Type은 [이 링크](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)에서 볼 수 있다.



### multipart/form-data

multipart/form-data는 HTML Form의 내용을 전송할 때 사용할 수 있는 문서 형식이다. 

`--`로 시작되는 문자열인 boundary로 구분되는 서로 다른 파트들로 구성되며, 각각의 파트는 그 자체로서 개체이며 자신만의 HTTP 헤더를 가진다. 파일 업로드 필드를 위한 헤더로는 `Content-Disposition`과 `Content-Type`이 있다.

Content-type Header에 boundary parameter를 포함해야하며 이 boundary parameter는 메세지 파트의 구분(multipart를 서로 구분해주는 역할)과 메세지의 시작과 끝을 나타내는 역할을 한다. 

첫번째 Boundary 이전의 Body는 MIME을 지원하지 않는 클라이언트를 위해 제공되는 영역이다.

boundary parameter에 들어갈 문자열을 프로그래머가 무작위 문자를 선택해서 작성한다. 이 때 보통 앞부분에는 WebkitFormBoundary... 등과 같이 프로그래머가 인식할 수 있는 문자를 삽입하며 뒷부분에 무작위 문자열을 붙여서 Boundary가  unique하도록 만들어 본문과의 충돌을 피한다. 



## HTTP 코드를 보며 이해해보자 

```http
POST /item HTTP/1.1
Host: camp-open-market-2.herokuapp.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="title"

귀여운 딸기
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="descriptions"

귀여운 딸기입니다.
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="price"

1000
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="currency"

KRW
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="stock"

10
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="images[]"; filename="Strawberry-removebg-preview.png"
Content-Type: image/png

(data)
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="password"

neph1234
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```



### Request Line

```http
POST /item HTTP/1.1
```

가장 첫 줄에는 Request Line이 적혀있는 것을 볼 수 있다.

POST는 HTTP Method 중 하나이며 더 많은 메서드에 대한 정보는 [이 링크](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)에서 확인할 수 있다.



### Request Headers 

```
Host: camp-open-market-2.herokuapp.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
```

Request Header에는 여러가지 Header가 선택적으로 들어갈 수 있으며 이 코드에서는 Host와 Content-Type Header가 들어가 있다.

Content-Type의 내용으로는 multipart/form-data와 boundary parameter가 전달된다.

boundary parameter로는 임의의 문자열이 전달되면 된다 (보통 swift에서는 UUID를 활용)



### Request Body

```
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="title"

귀여운 딸기
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="descriptions"

귀여운 딸기입니다.
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="price"

1000
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="currency"

KRW
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="stock"

10
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="images[]"; filename="Strawberry-removebg-preview.png"
Content-Type: image/png

(data)
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="password"

neph1234
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```



Request Body내의 하나의 단락의 시작은 boundary로 시작한다.

밑줄에는 Content-Disposition을 적어주고

blank line을 하나 삽입한 뒤

`name`에 명시된 공간에 들어갈 내용을 적어준다



즉, 아래의 코드가 하나의 단락이다.

```
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="title"

귀여운 딸기
```



주의할 점은 (data)라고 적힌 공간에는 문자열로 변환된 data가 들어있다. 

이미지의 문자열로의 변환은 `UIImageJPEGRepresentation`메서드를 통해 진행해줄 수 있다.



Body의 마지막에는 boudary를 적어주며, 꼭 따라야하는 것은 아니지만 관례적으로 boudary 뒤에 `--`를 붙여줌으로써 구분이 쉽도록 한다.



## Swift 코드를 통해 multipart/form-data 코드 만들기

처음 multipart/form-data 코드를 만드려하면 참 난감하다.

그래서 위에 작성한 multipart/form-data를 swift 코드로 변환해주는 함수를 작성하여 아래에 남겨두었다.

```swift
func convertTextField(key: String, value: String) -> String {
        var fieldString = "--\(boundary)\r\n"
        fieldString += "Content-Disposition: form-data; name=\"\(key)\"\r\n"
        fieldString += "\r\n"
        fieldString += "\(value)\r\n"
        
        return fieldString
    }
```

위 코드를 아래의 multipart/form-data 예제 단락을 통해 이해해보자

```
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="title"

귀여운 딸기
```

우선 parameter로 받아오는 key는 아래의 예시에서 `title`에 해당한다.

그리고 value로 넣어줄 값은 "귀여운 딸기"라는 String이다.

첫 줄에는 "--\(boundary)\r\n"라는 값을 넣어 단락의 시작을 알린다.

그 뒤로는 Content-Disposition과 value 값을 각각 fieldString이라는 문자열에 넣어 마무리한다.



> \n은 newline인 것을 알겠다지만 \r은 대체 왜 필요한지 의문이 들었다.
>
> 나와 같은 의문을 품을 독자를 위해 해당 의문에 대해 어느정도 답이 된 [글](https://m.blog.naver.com/taeil34/221325864981)을 첨부한다. 



### 참고한 링크

- HTTP 헤더의 종류들에 대해 잘 설명해놓은 글: [The most important HTTP headers for scraping](http://go-colly.org/articles/scraping_related_http_headers/)
- swift를 통한 URLSession Tutorial: [Uploading images and forms to a server using URLSession](https://www.donnywals.com/uploading-images-and-forms-to-a-server-using-urlsession/)
- mozilla의 Content-Type Document: [Content-Type](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)
- mozilla의 HTTP Methods Document: [HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
- `\r`은 왜써야할까? : [CR(\r), LF(\n)이란 무엇인가?](https://m.blog.naver.com/taeil34/221325864981)
