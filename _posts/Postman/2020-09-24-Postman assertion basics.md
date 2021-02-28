---
layout: post
title: 13. Postman Assertion Basics
image:
categories: Postman
tags:
  - Postman
  - API
---

- Parse the response body
- Write a test

## Response type
- json : `pm.response.json()`
- XML : `xml2Json(responseBody)`
- HTML : `cheerio(pm.response.text())`
- Plain-text : `pm.response.text()`
- CSV :  `csv-parse/lib/sync`

json이 제일 보편적으로 사용되는 api response형태이긴 함.



