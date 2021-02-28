---
layout: post
title: 5. Difference between Path and Query parameter
image:
categories: Postman
tags:
  - Postman
  - API
---

## Difference between Path and Query parameter

### Path parameter
어떠한 지정된 리소스. 다만 해당 리소스는 변할 수 있는 것.
포스트맨에서는 넣고싶은 path parameter 자리에 세미콜론을 집으면 `param` tab에서 입력할 수도 있다.

`https://api.trello.com/1/boards/:id`

### Query parameter
`?` 으로 시작하여 `key=value` 로 시작함. 여러개의 query parameter로 묶인다면, `&` 으로 묶이게됨.

- - - -
Query parameter는 순서를 스위칭해줄 수있으나, Path parameter는 순서를 바꿀 수 없음. 순서를 바꾸면 실행이 안된다거나, 실행결과가 다르게 나올 것임.



