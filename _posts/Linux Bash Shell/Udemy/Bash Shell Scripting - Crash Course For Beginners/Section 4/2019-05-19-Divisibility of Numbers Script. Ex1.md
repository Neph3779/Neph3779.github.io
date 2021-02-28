---
layout: post
title: Divisibility of Numbers Script. Ex1
image: /assets/images/posts/Linux Bash Shell/Section4/Divisibility of Numbers Script. Ex1/20322ACD-D05D-4AE5-898F-CE4CA92CDB49.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Divisibility of Numbers Script. Ex1

[Exercises Shell Scripting !]({% post_url 2019-05-19-Exercises Shell Scripting ! %})

```shell
#! /bin/bash
divisible() {
    local num=$1 #$1 is parameter
    echo " you wrote $num"

    reminder=$(( $num%2 ))
    echo "the reminder is $reminder"

    if [ $reminder -eq 0]; then
      echo "the number $num is divisible by 2 "
    fi
}
divisible 8
```

![](/assets/images/posts/Linux Bash Shell/Section4/Divisibility of Numbers Script. Ex1/20322ACD-D05D-4AE5-898F-CE4CA92CDB49.png)

- - - -
수학연산을 조건문 내부에 집어넣어서 표현가능.

```shell
if [ $(( $num%2 )) -eq 0 ]; then
	echo "the number $num is divisible by 2"
fi

```

- - - -
```shell
divisible() {
    local num=$1
    echo " you wrote $num"

    if [ $(( $num%2 )) -eq 0 ]; then
      echo "the number $num is divisible by 2 "
    fi
    if [ $(( $num%3 )) -eq 0 ]; then
      echo "the number $num is divisible by 2 "
    fi
    if [ $(( $num%5 )) -eq 0 ]; then
      echo "the number $num is divisible by 2 "
    fi

}

divisible 10


```

- - - -
이것은 매우 좋지 못한 처리방법. 더 좋은 방법이 있을 것임.

for문을 사용하는 것.
- - - -
### For loop
```shell
    for i in {2,3,5}
    do
      if [ $(( $num%i )) ]; then
        echo "the number $num is divisible by $i"
      fi
    done
```
이렇게하면 2, 3, 5 세 번동안 반복하며 체크할 수 있음.


