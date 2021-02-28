---
layout: post
title: Project 1 part B
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Project 1 part B

### Step - 4

- Check if grep found something.
  grep으로 아무것도 찾지못한다면, `empty String`이 return됨.

```shell
a=$(grep "spo" new.txt)

echo "$a" #return result String...

b=$(grep "spo" tries.sh)

echo "$b" #return empty String...
```



- -z : 빈 문자열인지 판단

 - grep -i : 대소문자를 구분하지 않음.
   grep -ni : 대소문자를 구분하지 않으면서 해당 문자열이 몇번째 행에 있는지 행번호 출력.



```bash
for myfile in *;
do
  if [ -f "myfile" ]; then
    #echo "$myfile"
    #grep -ni "spo" "$myfile"
    #if grep is fail, return empty string
    check=$(grep -ni "spo" "$myfile")
    if [ -z "$check" ]; then
      echo "EMPTY"
    else
      echo "FOUND"
    fi
else 
  echo "$myfile is NOT a file..."
  echo ""
fi

done
```
