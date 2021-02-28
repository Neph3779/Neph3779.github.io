---
layout: post
title: 19. Maximizing window and deleting cookies.
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## Maximize.
창의 최대화.

`driver.manage().window().maximize();`


## Deleting cookies.
쿠키의 삭제.

정적 컨텐츠의 갱신이나 자동완성 조합의 삭제등이 필요할 때에는 cookies를 삭제해주면 좋다.

`driver.manage().deleteAllCookies();`

##### Only one cookie?

`driver.manage().deleteCookieName(“”);`

##### Add the cookies

`driver.manage().addCookie(Cookie);`