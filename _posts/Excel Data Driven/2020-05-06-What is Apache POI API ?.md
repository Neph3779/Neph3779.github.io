---
layout: post
title: Introduction Excel Data Driven
image:
categories: "Excel-Data-Driven"
tags:
  - ExcelData
  - Java
---

대표적으로 testrail 등과 같은, testcase 작성 및 관리, 진척사항, 결과까지 한 번에 관리하는 툴도 있지만, 아직까지도 많은 곳에서 excel을 이용하여 테스트케이스들을 관리하고 있다.

이 것은 고객이 제일 접하기 쉬운 것이 excel 이라는 것에서부터 비롯되나, 각종 데이터 입력를 비롯하여 관리하기 쉽다는 점도 무시할 순 없다.

테스트를 진행할때, 예상 데이터들을 엑셀에 입력하고, 해당 엑셀파일을 읽어들인다면 어떠한 경우에는 좀 더 테스트 데이터를 관리하기 편할 수도 있다.

## What is Apache POI API ?

How to pull data from excel worksheets?
엑셀 파일을 읽어들이는 라이브러리.

## Maven Dependencies.

Maven 프로젝트를 생성하고 depenencie를 입력해야함.
Maven repository에 접속하여 `poi`와 `poi-ooxml`을 검색하여 적절히 기재해준다.



