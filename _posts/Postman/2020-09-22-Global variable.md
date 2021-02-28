---
layout: post
title: 7. Global variable
image:
categories: Postman
tags:
  - Postman
  - API
---



우측 상단의 눈을 클릭하여 Global variable을 사용할 수 있음.

- - - -
Request 안에서는 어떻게 사용할 수 있냐면, `{{variable}}` 로 사용할 수 있음. 2개의 꺽쇄

## Accessing variables in the scripts
`pm.globals.get("variable_key");`
`pm.globals.set("variable_key", variable_value);`


## Additional methods
`pm.globals.unset("variable_key");`
`pm.globals.clear();`



