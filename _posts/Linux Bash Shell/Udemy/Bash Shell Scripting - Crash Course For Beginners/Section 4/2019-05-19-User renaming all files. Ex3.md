---
layout: post
title: User renaming all files. Ex3
image: /assets/images/posts/Linux Bash Shell/Section4/User renaming all files. Ex3/B9661D08-5A9F-4911-A5AD-990D003DF887.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## User renaming all files. Ex3

[Exercises Shell Scripting !]({% post_url 2019-05-19-Exercises Shell Scripting ! %})

[Touch]({% post_url 2019-03-30-Touch %})

[Move]({% post_url 2019-03-30-Move %})

- - - -

![](/assets/images/posts/Linux Bash Shell/Section4/User renaming all files. Ex3/B9661D08-5A9F-4911-A5AD-990D003DF887.png)
이 상태에서 테스트
‘-a and

- - - -
```shell
#! /bin/bash

for i in {1..4}; do
  touch "${i}file.txt"
done

for i in {1..3}; do
  touch "myfile${i}.jpg"
done

echo "Creating test files..."
sleep 1
ls -l
echo ""

###################
choice=""
while [ "$choice" != "t" -a "$choice" != "j" ]; do
  #convert varable value to string
  echo "To rename .jpg file press j, To rename .txt press t:"
  read choice
  echo "You typed $choice"
done

read -p "Write prefix to add to thise files " pre
echo "the prefix is $pre"

if [ "$choice" == "t" ]; then
  for element in *.txt
  do
    mv ${element} ${pre}${element} #Change file name
  done
else
  for element in *.jpg
  do
    mv ${element} ${pre}${element} #Change file name
  done
fi

ls -l


```



