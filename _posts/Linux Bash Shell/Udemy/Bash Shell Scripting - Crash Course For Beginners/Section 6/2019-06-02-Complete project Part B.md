---
layout: post
title: Complete project Part B
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Complete project Part B

[Complete project Part A]({% post_url 2019-06-02-Complete project Part A %}) 에서 계속...

- - - -
우리는 어떠한 폴더의 내부에서 폴더만을 찾아 표시하는 것을 했다.
찾은 폴더의 안으로 들어가 다시 그 폴더의 내부를 표시하는 것은??

### within each folders, run a for loop each file.

```shell
for i in $(find . -type d);
do
  echo "this is folder $i"
  for myfile in $i/* ;
  do
    echo "thie file inside is : $myfile"
  done
  echo "************"
done
```

`for myfile in $i/* ;` 

여기에서 바깥족 반복문에서 얻은 결과(폴더)의 안쪽에 존재하는 모든 파일들(*) 의 이름을 출력하는 것.
위와 같은 경우, 어떤 폴더 안에 있는 폴더까지 내부 반복문의 출력에 걸려버린다.
중간에 if문을 넣어주어 **폴더라면, 출력할 수 있도록** 하는 조건을 걸어주어야 한다.

```shell

for i in $(find . -type d);
do
  echo "this is folder $i"

  for myfile in $i/* ;
  do
    if [ -f "$myfile" ]; then
      echo "thie file inside is : $myfile"
    fi
  done
  echo "************"
  echo " "
done

```

