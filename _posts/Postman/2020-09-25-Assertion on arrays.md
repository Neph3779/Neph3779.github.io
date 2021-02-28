---
layout: post
title: 16. Assertion on Arrays
image:
categories: Postman
tags:
  - Postman
  - API
---

## Assertion on arrays

array로 오는 response를 어떻게 검증할 것인지?

```javascript

let jsonData = pm.response.json();
let manufacturer = jsonData.filters[2];

pm.test("Manufacturer should not be allowed", function(){
	pm.expect(manufacturer.name).to.eql("MANUFACTURER");
	pm.expect(manufacturer.isAllowed).to.be.false;
});


```

배열인 만큼, 배열처럼 지정해주면 된다.

번호로 지정해서 변수로 꺼낸다음, 확인해보면 된다.


```javascript
let manufacturer;

for(let filter of jsonData.filters) {
	console.log(filter);
	if(filter.name == "Manufacturer") {
		console.log(filter);
	}
}
```

이렇게 for와 if을 조합해서 테스팅을 진행할 수 있다.


