---
layout: post
title: Shell Script에서 외부 파일을 읽어들이는 방법
image:
categories: Bash_Shell_Script
tags:
  - Bash Shell Script
  - Tip
  - 
---

## Shell Script에서 외부 파일을 읽어들이는 방법

보통 프로그래밍을 할 때, 많은 상수들 혹은 환경변수들을 하나의 상수 클래스, 혹은 열거체(enum?)를 이용해 관리할텐데요.
쉘 스크립트에서도 이러한 방법으로 시스템 환경변수가 아닌, 작업 시에 필요한 상수들을 하나의 파일에서 선언하여 관리를 할 수 있습니다.

이러한 환경변수, 상수들을 관리할 파일은 특별한 파일이 필요한 것은 아니고,
그냥 plain text로 생성해도 읽어들일 수 있습니다.

- - - -
```tex
## constantEnv.txt

GITHUB_PAGE_DIR="Documents/Github/Yoodahun.github.io"
POSTS="_posts"
WORK_DIRECTORY="_drafts"
```

저는 `constantEnv.txt` 라는 이름으로, 환경변수들을 관리해주고자 합니다.

```bash
#!/usr/bin/env bash
#testShell.sh
source jobenv.txt

echo "${GITHUB_PAGE_DIR}"
echo "${POSTS}"

```
테스트쉘에서는 환경변수 혹은 상수들을 선언한 파일을 `source`커맨드를 통해 읽어들입니다.

이후 그냥 사용하기만하면 됩니다.
- - - -
실행결과
![](/assets/images/posts/Linux Bash Shell/Shell Script/externalFileRead/9D2A4EF9-2B06-4C6C-B14A-9EAD353E81A6.png)
이런식으로, 텍스트파일에 선언한 변수들을 잘 읽어오는 것을 확인할 수 있습니다.



끝