---
layout: post
title: Project 1 part C
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Project 1 part C

### Step - 5

- Create a Folder and copy the Desired file in it

- **Don’t do make directory inside the for loop**
  	⇒ In this way, you will be creating this directory many time.
  - 반복문 안에 `mkdir` 을 하게되면 수많은 폴더가 생성되므로 주의할 것...

``` shell
mkdir ./hot-folder #make directory in present working directory
for myfile in *;
do

  if [ -f "$myfile" ]; then
    #echo "$myfile"
    #grep -ni "spo" "$myfile"
    #if grep is fail, return empty string
    check=$(grep -ni "spo" "$myfile")
    if [ -z "$check" ]; then
      echo "EMPTY"
    else
      echo "HOT FILE !!! FOUND"
      cp "$myfile" ./hot-folder
      echo "$myfile is copied into hot-folder !!!"
    fi
else
  echo "$myfile is NOT a file..."
  echo ""
fi
echo "------------------"

done
```
- when we found something, copy to folder.
  이 스크립트에서, grep으로 해당 파일을 찾는다면, 특정 폴더로 복사사키는 것.

- - - -

### Step - 6

- Append to the copied file, the number of the line where the hot world is found.
- To append something, using `>>`
  `echo " " >> hot-folder/"$myfile"`
  - 해당 파일에 제일 마지막줄부터 추가를 해주는 것.

```bash
# Step 6***********************************
mkdir ./hot-folder #make directory in present working directory
for myfile in *;
do
  if [ -f "$myfile" ]; then
    #echo "$myfile"

    #grep -ni "spo" "$myfile"
    #if grep is fail, return empty string
    check=$(grep -ni "spo" "$myfile")
    if [ -z "$check" ]; then
      echo "EMPTY"
    else
      echo "HOT FILE !!! FOUND"
      cp "$myfile" ./hot-folder
      echo "$myfile is copied into hot-folder !!!"
      echo " " >> hot-folder/"$myfile"
      echo "******" >> hot-folder/"$myfile"
      echo "$check" >> hot-folder/"$myfile"
    fi
else
  echo "$myfile is NOT a file..."
  echo ""
fi
echo "------------------"

done
# Step 6***********************************
```

해당 파일을 찾으면, 파일의 맨 마지막에 결과를 추가함.
결과 : 행의 숫자와, 그 행의 내용을 추가.
