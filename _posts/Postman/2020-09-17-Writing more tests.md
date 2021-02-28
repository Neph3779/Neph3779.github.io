---
layout: post
title: 4. Writing more tests
image:
categories: Postman
tags:
  - Postman
  - API
---

Response body에서 원하는 값이 들어있는지 테스트를 해보려면?

```javascript
pm.test("Your test name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.value).to.eql(100);
});

```

이 스니펫에서 조금 고쳐줘본다.

```javascript
pm.test("Board shoud be created", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.name).to.eql("my new board using API");
});

```

`jsonData` . jsonKeyName으로 지정하여 찾아볼 수 있음. `name` 키 값의 vlaue가 eql의 값이 일치하는지를 확인하는 것.

- - - -

Nested object
```javascript
pm.test("Board shoud be private", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.prefs.permissionLevel).to.eql("private");
});

```
