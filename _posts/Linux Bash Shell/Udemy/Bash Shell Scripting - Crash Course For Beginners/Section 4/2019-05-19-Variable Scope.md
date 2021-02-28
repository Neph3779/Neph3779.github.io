---
layout: post
title: Variable Scope
image: /assets/images/posts/Linux Bash Shell/Section4/Variable Scope/5B03B2FB-6732-42AF-8218-567B6F8CDA00.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---



## Variable Scope

```shell
#! /bin/bash
myFunction() {
  var1="Eggs"
  var2="Salad"
  echo " my variable inside the func is: $var1"
}

echo "start the program"
```

![](/assets/images/posts/Linux Bash Shell/Section4/Variable Scope/5B03B2FB-6732-42AF-8218-567B6F8CDA00.png)
- - - -

만약 var1을 펑션 밖에서 실행시키려 하면??

![](/assets/images/posts/Linux Bash Shell/Section4/Variable Scope/D4EFE3C3-9CC9-4AD5-B6DE-153109A97FD8.png)

**펑션의 밖이라도 변수의 값을 기억함.**

-> Shell에서의 모든 변수는  ***글로벌 변수***  임
- - - -
### 만일 local 변수로 쓰려고 한다면??
변수 이름 앞에 `local`을 추가 기재해야함.

```shell
myFunction() {
  local var1="Eggs"
  var2="Salad"

  echo " my variable inside the func is: $var1"

}

echo "start the program"

myFunction

echo "Outside : $var1"
```

![](/assets/images/posts/Linux Bash Shell/Section4/Variable Scope/957441D5-A019-4D29-B7EF-58D59D100510.png)

글로벌 변수로 인해 아주 복잡해질 수 있음.
-> 그렇기 때문에 펑션 내부에서는 로컬 변수를  사용하는 것이 좋음.

