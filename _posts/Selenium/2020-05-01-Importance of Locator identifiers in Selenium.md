---
layout: post
title: "02. Importance of Locator identifiers in Selenium"
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

## Importance of Locator identifires in Selenium

Selenium에게 어떠한 요소의 위치를 알려주기 위해서는 locator identifier로 요소를 알려줄 필요가 있다.

그래야 해당 요소에 접근해서 원하는 어떠한 동작을 할 수 있다.

해당 요소(Element)를 인식하기 위해서는 어떠한 태그, 아이디, 클래스 등의 attribute를 인식할 수 있어야함.

그렇다면 어떻게 이 정보들을 얻을 수 있을까?

→ Developer Tool에서 확인할 수 있음…!!

- - - -
Selenium에서 지원하는 locator는 다음과 같음

- ID
 - ClassName
- Name
- LinkText
- Xpath
- CSS

`getElement(By by)` 로 찾을 수 있음.
By에는 위 6개의 locator를 식별하는 메소드가 존재

- - - -
### ID, ClassName, Name, LinkText
Selenium에게 줄 수 있는 Unique한 요소의 정보
그러나 위 요소들의 값들은, 존재하지 않을 수도 있음.
LinkText는, 링크로 쓰이는 a tag의 텍스트를 찾아내서 하는 것임.
링크는 무조건 a태그가 쓰임.

class에는 스페이스가 들어있으면 안된다.

때문에 여러개의 class가 쓰일때는 특정 요소를 className으로만 특정짓기는 어렵다.

또한 요소는 Top-left에서부터 순차적으로 검색하여 찾아낸다.
순차적으로 찾아내어 나온 제일 첫 번째 요소에 대하여 다음 행위를 실행함

- - - -
### Xpath, CSS
정확하게 원하는 요소를 추출할 수 있는 방법.

Chrome에서는 CSS만 추출해낼 수 는 없음.

Chrome > console에서 $x(“”) 으로 xpath가 제대로 올바른지를 확인할 수 있음.
더블 쿼테이션 내부의 더블 쿼테이션은 싱글 쿼테이션으로 바꾸어야함.

---

### Identifying Object

- 테스트를 실행하기 전에 Path를 한 번 더 확인할 것.
- Css를 xpath와 같은 문법으로 하면 좋음. tagName[attribute=‘value’]
- Css Selector ‘#id .class 
  - CSS는 xpath보다 빠름.

- Find the unique attribute 
  -  이게 가장좋음. id는 모든 페이지에서 유일한 값을 지니고 있음.