---
layout: post
title: Change Permission
image: /assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 19.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## chmod

- 	Change Mode
- 	chmod (옵션)=(권한) (바꾸고자하는 파일명)
	- 	u 내가 오너라면
	- 	g 내가 게스트라면
	- 	o 둘다 아니라면
	- 	write, read, execute
- - - -

빠르게 권한을 바꾸고 싶다면??
![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 19.png)
해당 파일에 실행권한을 +(더하기) 한다는 것.

![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 20.png)
해당 파일에 실행권한을 -(빼기) 한다는 것.

- - - -
### Owner, Permission Change Folder

![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 21.png)
파일에 권한이 없어서 삭제를 하지 못함. 폴더도 마찬가지.

폴더와 폴더 내부의 파일의 권한을 재귀적으로 변경할 수 있는 방법은??


![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 22.png)

![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 23.png)
오너를 루트에서 나 자신으로 바꿈. 재귀적으로 내부 파일 및 폴더를 전부 바꾸었음.

---

![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 24.png)
![](/assets/images/posts/Linux Bash Shell/Section2/Change Permission/Pasted Graphic 25.png)
그룹을 바꿀때에도 재귀적으로 가능.