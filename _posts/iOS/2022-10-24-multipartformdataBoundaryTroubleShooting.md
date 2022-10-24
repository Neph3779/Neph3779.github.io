---
layout: post
title: "[Swift] multipart/form-data 트러블 슈팅"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Multipart/form-data

Multipart/form-data는 파일을 포함한 데이터를 전송할때 사용하는 데이터 형식이다.

Content-Type 헤더(키)에 값으로 `multipart/form-data; boundary={uuid}`를 지정하고

```http

Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```



위처럼 `--boundary`로 영역을 구분하여 값들을 넣어주면 되는 형식이다.



## 에러 발생 원인

구분시에 boundary 앞에 반드시 "--"를 붙여주어야 한다.

(마지막에는 --boundary--와 같이 끝에도 --를 붙여줘야 한다.)

이미지 형식이 잘못되었을때 서버에서 주는 에러 메세지와

내가 한 실수를 범했을때 주는 에러 메세지가 같아서 이미지 형식 문제, 이미지 인코딩 문제인줄 알고 꽤 헤맸다.



![image-20221024195300793](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221024195300.png)

postman에서는 boundary를 -------------처럼 긴 구분선으로 시작하도록 만들기 때문에

boundary 앞에 --가 더 붙은건지 아닌건지 한눈에 파악하기 어려웠다. (마지막에 붙는 --는 그나마 알아채기 쉬운편)

이 에러로 하루 정도는 날린 것 같아서 많이 아쉽다.. 