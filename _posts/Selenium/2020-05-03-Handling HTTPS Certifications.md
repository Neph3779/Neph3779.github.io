---
layout: post
title: 18. Handling HTTPS Certifications
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Handling HTTPS Certifications



Capabilities is customize chrome browser. Chrome 브라우저의 Setting을 변경하는 것.

```java
//general chrome profile.
DesiredCapabilities ch = DesiredCapabilities.chrome();
ch.acceptInsecureCerts(); //certificated.

ch.setCapability(CapabilityType.ACCEPT_SSL_CERTS, value);

//belongs to my local browser
ChromeOptions c = new ChromeOptions();
c.merge(ch);
WebDriver driver = new ChromeDriver(c);

```

DesiredCapabilites를 이용하여 필요한 설정을 해주고, 그것을 로컬 브라우저에 적용을 해주면 됨.

