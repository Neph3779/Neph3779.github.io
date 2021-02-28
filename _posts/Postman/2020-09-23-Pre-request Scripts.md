---
layout: post
title: 12. Pre-request Scripts
image:
categories: Postman
tags:
  - Postman
  - API
---



- Similar to test scripts, but with no assertions
- Ideal for making your requests dynamic
- Usually used in combination with variable

Request를 만들기 전에, 일정한 script를 사용하여 request의 정보를 만드는 것.

- - - -

Request구역에서 `Pre-request script` 를 클릭한다.

`pm.environment.set(“boardName”, parseInt(Math.random()*1000));`

테스트 코드와 별반다를 것 없이, 자바스크립트를 이용하여 작성해줄 수 있다.

```javascript
const boardName = getRandomBoardName();
pm.environment.set("boardName", boardName);

function getRandomBoardName() {
    return "My board name " + parseInt(Math.random()*1000);
}

```

함수도 지정해줄 수 있음.


