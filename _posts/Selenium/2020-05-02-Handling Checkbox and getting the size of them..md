---
layout: post
title: "07. Handling Checkbox and getting the size of them."
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Handling Checkbox and getting the size of them.

보통은 input에 타입이 checkbox인 경우임.
`<input type="checkbox" id="vehicle1" name="vehicle1" value="Bike">`

checkbox는 따로 메소드가 없이 클릭`click()`을 해주면되는데, 이것이 선택이 되어있는지 없는지를 확인하기 위해서

`isSelected()`가 쓰임.
Return True or false

- - - -
요소들의 개수를 셀때는,
findElements().size()를 사용.

`driver.findElements(By.cssSelector("input[type='checkbox']")).size()`

이렇게 하면 어떠한 한 페이지 내의 있는 모든 체크박스들의 개수를 확인할 수 있음.

