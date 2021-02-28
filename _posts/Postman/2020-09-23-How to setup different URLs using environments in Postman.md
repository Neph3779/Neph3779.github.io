---
layout: post
title: 11. How to setup different URLs using environments in Postman
image:
categories: Postman
tags:
  - Postman
  - API
---



실행하는 환경별로 URL을 다르게 설정해보기.
보통 주소는 hard-coding하긴 하는데, 환경에 따라 바꿀 수 있다면 시간을 허비하거나, 미스타이핑으로 고생할 일은 없을 것임.

이럴 땐 Environment와 Environment variable을 사용해보는 것.
- - - -
![](/assets/images/posts/Postman/How-to-setup-different-URLs-using-environments-in-Postman/8531CCE3-EE09-43AC-A112-98851E24F124.png)
environment를 각각 설정해주고, 같은 용도로 쓰이는 값들에 대하여, 같은 key이름으로 설정을 해주고 사용한다.
이때 environment만 바꾸어주면, 돌아가면서 쓸 수 있는 것이다.
key값은 환경이 바뀌어도 다 같은 값으로 선언하였기 때문에 굳이 수정해주지 않아도 된다.


