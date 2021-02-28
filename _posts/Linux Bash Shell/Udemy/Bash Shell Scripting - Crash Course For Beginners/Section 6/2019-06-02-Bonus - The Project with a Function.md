---
layout: post
title: Bonus - The Project with a Function
image:
categories: 
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## Bonus - The Project with a Function

[Complete project Part C]({% post_url 2019-06-02-Bonus - The Project with a Function %}) 에서 계속…

- - - -
중요한 로직부분을 함수화하여 불러오는 것을 해봄.
```bash
#!/usr/bin/env bash
#function
get_file() {
 local str="$1"
 local fil="$2"

 # echo "the String you are Looking for is: $str"
 # echo "the file is you are searching is $fil"

 #grep -ni $str $file

 check=$(grep -ni "$str" "$fil")

 if [ -z "$check" ]; then
   echo "### EMPTY !! ###"
 else
   echo " FOUND !!! "

   name_file=$(basename "$fil")
   #echo "just filename is: $name_file"
   cp "$fil" hot-folder/"$name_file"
   echo "$myfile is copied into hot-folder !!!"
   echo " " >> hot-folder/"$name_file"
   echo "******" >> hot-folder/"$name_file"
   echo "$check" >> hot-folder/"$name_file"
 fi

}
```

그리고 해당 함수를 호출하는 본 프로그램을, 함수 아랫부분에 생성.
``` bash
#######   Whole Program ####################
rm -r /hot-folder
mkdir ./hot-folde

for i in $(find . -type d);
do

if [[ $i != "./hot-folder" ]]; then
  #statements
  echo "########### THE FOLDER IS $i ############"
  for myfile in $i/*;
  do
    if [[ -f "$myfile" ]]; then
      #statements
      get_file "spo" "$myfile"
    fi
  done
fi
echo "-----------------"
done
#######   Whole Program ####################
```


