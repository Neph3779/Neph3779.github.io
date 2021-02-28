---
layout: post
title: Project 1 part A
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Project 1 part A

### Step - 1

- Create a loop on all items contained in a folder
- 일단 프로젝트를 하기에 앞어서 아래의 커맨드를 실행하여 프로젝트 준비를 한다...
  - 왠지모르게 atom의 터미널에서 touch를 하면 실행권한이 부여가 되질 않았다.
    1. `touch ourscript1.sh`
    2. `chmod +x outscript1.sh`

``` bash
#! /bin/bash

for myfile in *;
do
  echo "$myfile"
done
```

![](/assets/images/posts/Linux Bash Shell/Section5/Project 1 part A/970EDC98-DA96-4CB1-A44D-07A1223AE807.png)
-> 루프를 돌며 현재 위치의 모든 내용물들의 이름을 출력하는 것.

- - - -
### Step - 2

- Create a loop only on files contained in a folder
- 파일만 있는 폴더? 루프문을 수정하여 **파일**만 출력하도록 하기.

```bash
#! /bin/bash

for myfile in *;
do
  if [ -f $myfile ]; then
    echo "$myfile"
  else
    echo "$myfile is NOT a file..."
    echo " "
  fi
done

```
- 폴더나 파일명에는 스페이스가 들어가는데 그것을 핸들링해주어야함.
  - `if [ -f "$myfile" ]; then`더블콤마로 감싸기.



- - - -
### Step - 3

- Find a specific word in all files

```bash
#! /bin/bash

for myfile in *;
do
  if [ -f "$myfile" ]; then
    echo "$myfile"
    grep -n "dum" "$myfile"
  else
    echo "$myfile is NOT a file..."
    echo " "
  fi
done

```
- grep을 이용하여 `$myfile`들에 대해 해당 단어를 검색.

- -n 몇번째 라인인지 출력

  

