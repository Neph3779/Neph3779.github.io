---
layout: post
title: 8. Environment Variable
image:
categories: Postman
tags:
  - Postman
  - API
---

## Environment Variable

- Similar to global variables but with a narrower scope
- Ideal for switching between different setups
- Same usage in the request builder as with global variables

우측 상단의 눈동자를 통해 environment variable을 추가해준다.
이 env variabled은, **특정 Collection** 에서만 사용할 수 있다. environement를 설정해야만 사용할 수 있다.

다른 컬렉션에서는 사용할 수 없다.

즉 특정 경우에만 사용하고 싶은 변수들이 있는 경우, 나누어서 관리하면 좋다.

## Accessing variables in the scripts
`pm.environment.set(“variable_key”, variable_value);`
`pm.environment.get("variable");`
`pm.environment.unset("variable");`
`pm.environment.clear();`




