---
layout: post
title: 17. Handling table grids in Webpage
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



![](/assets/images/posts/Selenium/HowToHandleTableGridInWebpage01.png)

R의 점수들을 모두 집계하여  Total과 같은지 비교해볼 것.
어떠한 공통된 CSS를 찾아내어 해당 값들을 더할 것.
그러나 해당 페이지에 똑같은 표들이 여러개 있다면 중복의 가능성이 있음.

특정 테이블을 찾아낼 필요가 있다. [Driver scope]({% post_url 2020-05-03-Driver scope %})
그후 어떠한 행의 몇번째 cell인지 확인한 다음 해당 cell의 데이터만 찾아내는 것은?

- - - -
`By.cssSelector(".cb-col.cb-col-100.cb-scrd-itms div:nth-child(3)" )`

CSS의 지정방법으로는, 띄어쓰기로 지정하게되면 바로 아래의 자식태그를 지정하게되는 것.

```java
 WebElement table;
        table = driver.findElement(By.cssSelector(".cb-col.cb-col-100.cb-ltst-wgt-hdr"));

        List<WebElement> row = table.findElements(By.cssSelector(".cb-col.cb-col-100.cb-scrd-itms div:nth-child(3)" ));
        Thread.sleep(3000);
        System.out.println( row.size());
//        System.out.println( row.get(0).findElement(By.cssSelector(".cb-col.cb-col-8.text-right.text-bold")).getText());

        int score = 0;
        int total = 0;
        for( int i = 0; i < row.size()-2; i++) {
            String sc = row.get(i).getText();
            score += Integer.parseInt(sc);
        }
        total = Integer.parseInt(driver.findElement(By.xpath("//*[@id=\"innings_1\"]/div[1]/div[13]/div[2]")).getText());
        Thread.sleep(2000);
        score += Integer.parseInt(driver.findElement(By.xpath("//*[@id=\"innings_1\"]/div[1]/div[12]/div[2]")).getText());

        System.out.println(total);
        System.out.println(score);

        Thread.sleep(3000);
        driver.quit();
```






