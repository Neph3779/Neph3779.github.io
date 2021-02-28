---
layout: post
title: 2. First API Test in Postman
image:
categories: Postman
tags:
  - Postman
  - API
---



- Commonly just a simple assertion that from response value
- one request can have multiple tests
- tests only run AFTER the request completed

  - - -


일단 Request 구역의 **Test** 탭을 사용하자.
해당 탭에는 스니펫이 있어서 이미 작성된 테스트 코드를 사용할 수 도 있다.
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

```

- pm test : function for writing test specifications
- works in a non-blocking way in case of errors
- can include multiple related assertions
	- first parameter is the test name, as a String
	- The second parameter is so-call callback function which is called when the underlying execution (in this case assertions) has finished
	- pm.response - is the response assertion API and can make assertions on the response object (status code, headers, body)