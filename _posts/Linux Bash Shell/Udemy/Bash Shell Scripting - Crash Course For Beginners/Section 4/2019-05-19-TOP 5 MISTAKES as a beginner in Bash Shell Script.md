---
layout: post
title: TOP 5 MISTAKES as a beginner in Bash Shell Script
image: /assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 20.png
categories:
tags:
  - Bash Shell Script
  - Udemy
  - Bash Shell Scripting - Crash Course For Beginners
---

## TOP 5 MISTAKES as a beginner

1.

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 20.png)
if문의 조건에서 괄호에 스페이스공간을 두지 않았기때문.
또한 조건식에도 스페이스를 넣어주어야함.

---

2.

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 21.png)
변수에 값을 대입할때는 스페이스를 넣으면 안됨

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 22.png)
조건식에서는 변수를 더블쿼테이션으로 묶어주어야, 공백문자를 포함한 인자를 하나의 인자로 인식함.



---

3.

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 23.png)
파일명을 읽을때에도 마찬가지.
second trip이라는 두개의 단어로 구성된 파일이름을 인자로 넘겨준다면, 두개의 파라미터로 인식하고 에러를 일으키게됨.
변수를 더블쿼테이션으로 묶어준다면 좋음.

---

4.

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 24.png)
실행이 안됨. 



![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 25.png)

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 26.png)

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 27.png)

이렇게 구분을 해주어야함.

---

두개의 변수를 묶어서 출력하고싶으면??

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 28.png)

---

5.

![](/assets/images/posts/Linux Bash Shell/Section4/TOP 5 MISTAKES as a beginner/Pasted Graphic 30.png)
싱글쿼테이션이라면 a가 나오지않음. $a라는 문자로 나옴.
더블쿼테이션을 감싼다면 제대로 변수의 내용이 출력됨.




