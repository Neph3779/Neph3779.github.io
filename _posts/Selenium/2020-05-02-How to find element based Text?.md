---
layout: post
title: "04. How to find Element based Text?"
image:
categories: Selenium
tags:
  - Selenium
  - XPath
---

How to find element based Text?

---

"//*[text()='日本語']"

**텍스트만 이용해서도 검색가능하다.**

- 그러나 모든 방법을 이렇게 하는 것은 좋지않음.
- Xpath 로 특정짓기가 어려울 때에는 텍스트는 항상 변할 수 있는 가능성이 있음.
- 또한 텍스트에는 보이지 않는 공백이 들어가있을 수 있기 때문에 특정하기가 어려운 상황이 많음. 
  - 일본어와 같은 언어의 경우에는 전각처리로 인하여 공백으로 보이나 공백이 아닌 경우도 있음.



