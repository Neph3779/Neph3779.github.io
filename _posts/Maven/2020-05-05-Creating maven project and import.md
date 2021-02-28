---
layout: post
title: Creating maven project and import
image:
categories: Maven
tags:
  - Maven
  - Java
---

Maven 프로젝트를 생성하게 되면 User의 home directory에 `.m2/repository` 라는 폴더가 자동으로 생성되고 jar파일을 다운받게 된다.

`/Users/yoodahun/.m2`

프로젝트 최상위 루트레벨에 `pom.xml` 가 있을텐데, 파일 내부에 `dependency` 태그로 감싸서 기입해주면 알아서 다운로드받게됨.

- - - -
## For TestNG

Maven project에서는 `Maven surefire` 라는 플러그인을 설치해주어야지 전체 테스트가 진행됨.
Maven 공식 사이트의 usage에서 확인 할 수 있는 아래 코드를 `pom.xml`에 붙여넣기.

```xml
	<build>
	    <pluginManagement>
	      <plugins>
	        <plugin>
	          <groupId>org.apache.maven.plugins</groupId>
	          <artifactId>maven-surefire-plugin</artifactId>
	          <version>3.0.0-M4</version>
	        </plugin>
	      </plugins>
	    </pluginManagement>
  </build>

```

`build`  태그 내부에 `pluginManagement` 에서 관리. 즉 하나의 플러그인.