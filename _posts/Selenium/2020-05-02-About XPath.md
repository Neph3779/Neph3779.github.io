---
layout: post
title: "03. About XPath"
image:
categories: Selenium
tags:
  - Selenium
  - XPath

---

Selenium에서 element 를 식별하는 방법은 여러가지가 있으나, 일반적으로 Xpath가 많이 쓰인다.

[Importance of Locator identifiers in Selenium]({% post_url 2020-05-01-Importance of Locator identifiers in Selenium %})

---

## Identifying Xpath

Xpath can generate using WebBrowser.
그러나 100% 믿을 수는 없기 때문에 Customize한 Xpath를 작성해야할 필요가 있음.

Webbrowser 마다 xpath를 읽어들이는 게 다르기 때문에 조금씩 차이가 있음.

### 어떻게 Xpath를 확인할 수 있나?

브라우저의 개발자모드에서 원하는 요소를 오른쪽 클릭하여 확인할 수 있음.
Copy > Copy Xpath

----

## Generating customized path from html attributes

```html
<input class="input r4 wide mb16 mt8 username" type="email" value="" name="username" id="username" aria-describedby="error" style="display: block;">
```

※chrome console에서는 일단 더블쿼테이션으로 입력한다음, 콘솔상에서 싱글 쿼테이션으로 바꿔야지 적용이되는 문제가 있음. 왜인지는 잘 모르겠음.

Input = tag
Other is Attributes.

- class : text
- name : text
 - id : text
- style ~~

##### xpath

//tagName[@attribute=‘value’]

##### CSS

tagName[attribute=‘value’]

---

### Regular Expression

##### Xpath

//tagName[contains(@attributes, ‘value’)] 
//input[contains(@name, “username”)]

##### CSS

tagName[Attribute*=‘value’]

---

### 다시 한 번 유의해야할 것 들.

- Evey object may not have ID, className or name, So, Xpath and CSS Preferred
- Alpha numeric id may vary on every refresh- **check**
- Confirm the link object with anchor “a” tag
- Classes should not have spaces
  -  Compound classes cannot be accepted
- Multiple values 
  - Selenium identifies the first one
  -  Scans from top left
- Double quotes inside double quotes are not accepted
  - Use single quotes
- Xpath/CSS  can be defined in n number of ways
- Rightclick copy on blue highlighted html code to generate xpath
- Firepath depreciated from firefox-
- when xpath starts with html
  - Not reliable
  - Switch browser to get another one
- There is no direct way to get CSS in chrome. You will find it in tool bar
- Degrade browser to less firefox 55 to ge Firepath
- $(“”) 
  -  for css
-  $x(“”)  
  - xpath
- //tagName[@attribute=‘value’]  - xpath syntax
- tagName[attribute=‘value’]  -CSS  
- tagName#id- CSS   
- tagname.classname- CSS
- //tagName[contains(@attribute,’value’)]  - xpath regular expression
- tagName[Atrribute*=‘value’] - Css regular expression

----

## Difference between Relative and Absolute XPath?

### Relative Path

정확하게 원하는 요소를 찾아낼 수 있음.
부모 요소에 의존적이지 않음.

### Absolute Path

부모들 찾고 그 부모의 자식요소들을 찾아가 모든 세대를 선언하는 것이 절대 패스.
부모/자식/자식 으로 이어짐.