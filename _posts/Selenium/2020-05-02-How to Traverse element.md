---
layout: post
title: "05. How to Traverse Element"
image:
categories: Selenium
tags:
  - Selenium
  - XPath
---

How to Traverse element

### Sibling Element

바로 같은 레벨의 형제요소 들을 선언하여 찾아가는 것.

1. 절대경로로 찾아내는 것.
   - Example : //*[@id="body-inner"]/p[23]
2. ~/following-sibling::element[number]
  1. 이때 바로 아래 형제요소는 1번부터 시작한다.
    //*[@id="tsf"]/div[2]/div[1]/div[3]/center/input[1]

### Back to Parent from child element

~/parent::element

XPath는 거꾸로 거슬러올라갈 수 있다.
CSS는 이것이 불가능하다.

---

### Define Xpath for parent and traverse level

//[@id="tsf"]/div[2]/div[1]/div[1]
//[@id="tsf"]/div[2]/div[1]/div[1]/div

child를 찾아감.
//tagName[@attirbute=‘’]/tagName

Child가 여러개일때는 괄호에 몇번째 자식인지 번호를 지정함.
//tagName[@attirbute=‘’]/tagName[?]