---
layout: post
title: 1. How to create a request in Postman
image:
categories: Postman
tags:
  - Postman
  - API
---

`requestbin.com` 에서 자신만의 api를 만들어서 테스트해볼 수 있음.


![](/assets/images/posts/Postman/How-to-create-a-request-in-Postman/E00F1891-E21D-4399-B86B-837AB7486461.png)

이렇게 무엇을 보내었는지 확인해볼 수 있음.

- - - -
Param탭에서, 굳이 URL에 query parameter를 넣지않아도 편하게 변경할 수 있음.
만약 value값에 space를 넣고싶다면, encodeURIComponent라는 옵션을 사용해서 변경해주어야 할 필요가 있다.

- - - -
한 번 입력한 URL은 나중에 다시 입력할 때 자동완성 목록에 표시해준다.

## Path parameter
파라미터에 좀 더 의미를 부여해줄 수 있다.

> 3. 그럼 Path Variable과 Query Parameter를 각각 언제 사용해야 하는가?  
> 만약 어떤 resource를 식별하고 싶으면 Path Variable을 사용하고,  
> 정렬이나 필터링을 한다면 Query Parameter를 사용하는 것이 Best Practice이다.  
> 
> /users  # 사용자 목록을 가져온다.  
> /users?occupation=programer  # 프로그래머인 사용자 목록을 가져온다.  
> /users/123  # 아이디가 123인 사용자를 가져온다.  
> 또한, 기본적인 CRUD 기능을 위해서 또 다른 URL이나 query parameter를 정의할 필요는 없다.  
> 대신 원하는 기능에 맞게 HTTP 메소드를 바꾸어야 한다.  
> 
> /users [GET] # 사용자 목록을 가져온다.  
> /users [POST] # 새로운 사용자를 생성한다.  
> /users/123 [PUT] # 사용자를 갱신한다.  
> /users/123 [DELETE] # 사용자를 삭제한다.  
> 그렇다! 거의 모든 CRUD 프로세스를 추가적인 endpoint(예를 들어 users/create) 또는  
> query parameter(예를 들어 users?action=create) 없이 수행할 수 있다. 게다가 단순하고 예측 가능하다.  

`user/:userid/contract/:contracted`
와 같이, 세미콜론으로 들어갈 값의 변수이름을 지정해줄 수도 있음.

- - - -
## Header
request의 한 종류. request의 정보를 나타낸다고 생각하면 될 듯. 이미 많은 양이 미리 선언되어있으나, 필요하다면 자기 자신만의 header를 생성하는 것도 가능하다.

![](/assets/images/posts/Postman/How-to-create-a-request-in-Postman/F8E4ADD2-B082-4E18-B531-B69DFC7688E9.png)


헤더는 헤더탭의 우측에 presets에서 header의 프리셋을 따로 관리할 수 있음.

- - - -
## Body
바디의 종류는 몇가지가 있는데, `form-data`, `x-www-form-urlencoded` 는 비슷함. 서버에 전달하는 값.

Restful API에서 form-data는 바람직하진 않음.
![](/assets/images/posts/Postman/How-to-create-a-request-in-Postman/DBFFF771-0234-4627-A79C-F1209F876FCD.png)

Content-type과 body의 형태가, body의 종류에 따라 조금씩 다름.

binary를 선택하였을때는, 어떠한 파일을 전송할 때 사용. Form-data 를 선택했을 때에도 key값의 종류를 text가 아닌 file로 지정하면, value값을 어떠한 파일을 업로드해볼 수도 있다.








