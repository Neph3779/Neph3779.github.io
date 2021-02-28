---
layout: post
title: Maven Error
image:
categories: Maven
tags:
  - Maven
  - Java
---



자세히는 기억이 안나는데, 인코딩 어쩌구 저쩌구 하면 아래와 같이, `properties`  태그로 감싸서 넣어준다. `build` 태그 바로 아래인듯.

컴파일 버전이 문제이긴 한데, 자바 13을 쓰고있어서 1.13으로 지정해주었더니 제대로 안돌아갔다. 자세히는 아직 잘 모르겠다.

```xml
<!— Build Encoding UTF-8 Setting -->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
</properties>

```