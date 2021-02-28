---
layout: post
title: Counting of .txt Files Script. Ex2
image: 
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

# Counting of “.txt” Files Script. Ex2

[Exercises Shell Scripting !]({% post_url 2019-05-19-Exercises Shell Scripting ! %})

#### ./*
현재 위치의 파일들을 전부 읽어다가 몇개인지 숫자로 파악.
그러나 총 개를 알 수가 없음.
변수 하나를 만들어서 하나씩 증가시키는 작업을 해줘야함.

---

txt파일의 갯수만 알려면??
`for item in ./*txt` 
txt파일만 체크.

```shell
#! bin/bash
fileCheck() {
  number=0
  for item in ./*.txt
  do
    number=$(( number+1 )) #increment for counting files
    #echo "$number"
  done
}

echo "hello!"
fileCheck
echo "the number if items is: $number"
read -p "type Enter if you want to open all txt files" input

echo "you typed $input"
if [ -z "$input" ]; then # Is input Empty String?
    open *.txt
else
    echo "you did not press Enter!"
fi

```


