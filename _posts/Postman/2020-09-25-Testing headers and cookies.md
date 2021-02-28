---
layout: post
title: 17. Testing headers and cookies
image:
categories: Postman
tags:
  - Postman
  - API
---



## Testing headers and cookies

## Headers
This is how you retrieve a header from the response:
`pm.response.headers.get(‘X-Cache’)`

 and in a test:
**Header exists**: `pm.response.to.have.header(X-Cache’);`

**Header has value**:
`pm.expect(pm.response.headers.get(‘X-Cache’)).to.eql(‘HIT’);`

- - - -

## Cookies
cookies는 웹브라우저에 임시적으로 저장해놓은 유저의 정보같은 것. 서버는 이 쿠키를 보고 이전에 접속한 유저인지 다시 확인하고 상태를 유지시켜주는 역할을 한다.

In a similar fashion you can test cookies as well.
**Cookie exists:**
`pm.expect(pm.cookies.has(‘sessionId’)).to.be.true;`

**Cookie has value:**
`pm.expect(pm.cookies.get(‘sessionId’)).to.eql(’ad3se3ss8sg7sg3’);`


