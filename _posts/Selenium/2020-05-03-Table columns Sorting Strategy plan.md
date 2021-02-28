---
layout: post
title: 21. Table columns Sorting Strategy 
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



어떠한 테이블의 컬럼값이 어떻게 정렬되어있는지 확인해야하려면…???



2번째 컬럼만 필요하다면?
`tr td nth-child(2)` 로 지정해줄 수 있음.

테이블에 얻은 값들을 ArrayList에 저장하고
그 값을 한 번 복사하여 다른 arrayList에 저장한다음  sort를 진행함.
그러고는 테이블에서 얻은 값이 저장된 list와 정렬된 값이 저장된 list를 비교해보는 것.

```java
List<WebElement> td; //tr td nth-child(2)
td = driver.findElements(By.cssSelector(“tr td:nth-child(2)”));

ArrayList<String> originalList = new ArrayList<String>();
ArrayList<String> sortedList = new ArrayList<String>();

for (WebElement element : td) {
    System.out.println(element.getText());
    originalList.add(element.getText());
    sortedList.add(element.getText());
}
Collections.sort(sortedList);

```

sorting에는 `Collenctions` 을 사용함.

즉, 페이지에서 얻은 데이터들과, 해당 데이터들을 다시 정렬해봄으로써 페이지의 정렬 상태를 확인해보는 것.



---

역순정렬을 확인할 때에는 한 번 정렬해주고 뒤집어주어야함.
```java
Collections.sort(sortedList);
Collections.reverse(sortedList);
```


