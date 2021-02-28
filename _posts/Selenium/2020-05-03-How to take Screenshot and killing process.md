---
layout: post
title: 20. How to take Screenshot
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



### Take Screenshot
```java
//Screenshot
        byte[] src = ((TakesScreenshot)driver).getScreenshotAs(OutputType.BYTES);
        try {
            FileOutputStream fos = new FileOutputStream(System.getProperty("user.dir") + "/screenshot.jpg");
            fos.write(src);
            fos.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

```

User.dir는 보통 프로젝트가 위치한 폴더이다.

스크린샷의 이름 같은 경우에는, 어떠한 경우마다 이름을 다르게 지정해줘야할 필요가 있다.

- - - -
FileUtils.copyfile(src, new File(filepath)) 로도 할 수 있으나, Selenium 3.9이상부터는 직접 패키지를 다운로드해야함
[Commons IO – Download Apache Commons IO](https://commons.apache.org/proper/commons-io/download_io.cgi)

