---
layout: post
title: Command Substitution - Expansion
image: /assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 7.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

*$*를 익스펜션이라고 함. 변수를 넓게 확장하는 느낌??
	
	
![](/assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 7.png)
괄호로 감싸준 ls.
d를 실행시키면 ls의 결과가 d에 저장되는 것.

![](/assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 8.png)
**커맨드의 결과가 아닌 커맨드의 결과를 문자형식으로 그대로 출력하고 있음**

----

![](/assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 9.png)
현재 루트를 변수에 저장하고 나중에
![](/assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 10.png)
이런식으로 사용할 수 있음

![](/assets/images/posts/Linux Bash Shell/Section2/Command Substitution - Expansion/Pasted Graphic 11.png)
백틱스? 뒤로간 쿼테이션도 쓸 수도 있지만, 그다지 추천하진 않음.

