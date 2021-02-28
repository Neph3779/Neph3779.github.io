---
layout: post
title: 10. Understanding Variable Types
image:
categories: Postman
tags:
  - Postman
  - API
---

## Understanding Variable Types

## Variable Scopes
Global > Collection > Environment > data > local

만일 서로 다른 scope에 같은 이름의 변수가 존재한다면, 제일 가까운? 제일 좁은 scope의 변수부터 할당함.
변수 위에 마우스 오버를 해보면 알 수 있음.

## Which Variable Type to use?
왜 scope가 나뉘어져있는지를 알아야함.

### Global Variable
	- General Purpose : variable, ideal for quick results and prototyping
	- Avoid using as much as possible
	- Clear / Remove variables once you do not need them
	- Get variables in scripts using the scoped getter
		- `pm.variables.get()`
		- postman이 알아서 제일 가까운 scoped variable을 찾아서 할당해줌.
글로벌 변수를 자주 써버릇하면 엄청 많아져서 헷갈리게됨.
필요가 없어지면 클리어나 삭제를 해주어야함.

### Environment Variables
	- Ideal when working with different servers.
		- collection을 다른 서버에서 실행시킬 때 좋음.
	- Good alternative to global variables.
	- Storing environment specific information
	- URLs, authentication, credentials
	- Passing data to other requests.
	- Clear / Remove variables once you do not need them.
	- Avoid mixing with global variables
	- Get variables in scripts using the scoped getter
		- `pm.variables.get()`

### Collection Variables
	- Tied to a collection
	- Cannot be updated using scripts
	- Ideal for storing some constants that do not change during the execution
		- 스크립트 진행중 바뀌지 않는 상수를 저장하는 것?
	- Any values / constants that do not change
	- URLs / authentication / credentials if only one environment exist
	- Eliminate duplicate variables in environments
	- Limited usage as they cannot be created / updated with scripts
		- 스크립트에 의해 변하지 않는 값들에 대해서만 한정적으로 사용하는 것이 좋음
	- Avoid, unless you only need to define constants
	- Get variables in scripts using the scoped getter
		- `pm.variables.get()`

### Data Variables
	- Used when working with multiple data-sets
	- Exist only during the execution of an iteration
		- 일시적인 변수?
	- Can only be set from a CSV or a JSON file





