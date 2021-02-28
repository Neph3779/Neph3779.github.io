---
layout: post
title: 15. Assertion
image:
categories: Postman
tags:
  - Postman
  - API
---



```javascript
pm.test("Your test name", fucntion() {
	pm.expect(100).to.eql(101, 'must equal 101');
});
```

에러메세지를 내가 직접 지정할 수 있음. `eql` 의 두번째 파라미터.

- - - -

Json response를 통째로 eql 해볼 수도 있음.

`pm.expect(a).to.equal(b, '2')`
Equal 은 같은 오브젝트인지를 체크함. 
Eql 은 같은 값인지를 체크함.

- - - -
## Reverted assertion
`pm.expect(a).to.not.eql(b)`

같지 않다는 것을 확인함.

- - - -
Boolean 의 비교도 가능함.

`pm.expect(true).to.be.true;`
`pm.expect(null).to.be.null;`
`pm.expect(undefined).to.be.undefined;`
`pm.expect([]).to.be.empty;`
`pm.expect([].length).to.eql(0);`
`pm.expect([1,2,3]).to.include(2);`
`pm.expect(2).to.be.oneOf([1,2,3])` // 1,2,3중에 2가 들어있기 때문에 true
`pm.expect(‘John Doe’).to.match(/^John/)` regx를 사용하여 확인.

위와 같이 여러가지 패턴의 비교가 가능하다.

undefined와 null은 다른 것임.




