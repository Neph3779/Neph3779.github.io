---
layout: post
title: Complete project Part A
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Complete project Part A

![](/assets/images/posts/Linux Bash Shell/Section6/Complete project Part A/831CF834-4A73-4E22-B2E5-DE471A80C89F.png)

무엇인가를 재귀적으로 찾는 문제.
어떠한 폴더 안에 폴더가 있으면, 그 폴더 안으로 들어가 다시 내부를 찾는 그런 것.

폴더 내부 안에 폴더를 찾는 것?

[Find]({% post_url 2019-05-05-Find %})

```
find . -type d
```
- d : Directory

```shell
f=$(find . -type d)
for i in $f;
do
  echo "this is folder $i"
done

```


