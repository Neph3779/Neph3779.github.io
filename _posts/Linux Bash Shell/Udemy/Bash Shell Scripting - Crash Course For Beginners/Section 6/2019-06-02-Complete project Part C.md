---
layout: post
title: Complete project Part C
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Complete project Part C

[Complete project Part B]({% post_url 2019-06-02-Complete project Part B %}) 에서 계속…

- - - -

### We carefully add the previous script within this loop

이전 프로젝트에서 썼던 내용을 가져와서 해보기.
``` bash

for i in $(find . -type d);
do
  echo "************* FOLDER $i **************"

  for myfile in $i/* ;
  do
    if [ -f "$myfile" ]; then
      echo "thie file inside is : $myfile"
      ####################################
      check=$(grep -ni "spo" "$myfile")
      if [ -z "$check" ]; then
        echo "EMPTY"
      else
        echo "HOT FILE !!! FOUND"
        cp "$myfile" ./hot-folder
        echo "$myfile is copied into hot-folder !!!"
      fi
      ####################################
    fi
  done
  echo "************"
  echo " "
done

```
이대로는 문제가 있는데, 각 폴더안에 `hot-folder`라는 폴더가 없으면, 해당 이름을 가진 파일을 만들어 내용을 복사해버린다.
일단은 해당 폴더를 만들어두고 시작하는 것이 좋음.

- - - -
```bash
#!/usr/bin/env bash

for i in $(find . -type d);
do
if [ $i != "./hot-folder" ]; then

  echo "************* FOLDER $i **************"

  for myfile in $i/* ;
  do
    if [ -f "$myfile" ]; then
      echo "thie file inside is : $myfile"
      ####################################
      check=$(grep -ni "spo" "$myfile")
      if [ -z "$check" ]; then
        echo "EMPTY"
      else
        echo "HOT FILE !!! FOUND"

        name_f=$(basename $myfile)

        cp "$myfile" ./hot-folder/"$name_f"
        echo "$myfile is copied into hot-folder !!!"
        echo " " >> hot-folder/"$name_f"
        echo "******" >> hot-folder/"$name_f"
        echo "$check" >> hot-folder/"$name_f"
      fi
      ####################################
    fi
  done
  echo "************"
  echo " "
  fi
done

```
커다란 if를 추가하였는데, 폴더들 중에 제일 아래 있는 `hot-folder`의 내부는 검사하지 않는 것.

스크립트의 내용으로는, 원하는 문자열을 가진 파일을 찾게되면,
그 파일의 파일이름을 `basename`을 이용하여 획득.
해딩 쉘이 실행되는 계층의 `hot-folder`내부에 해당 파일을 복사하고, 그 파일 내부에 
원하는 문자열의 내용을 추가로 기재하는 것.

