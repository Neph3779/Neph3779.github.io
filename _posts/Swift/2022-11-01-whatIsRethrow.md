---
layout: post
title: "rethrow는 무슨 기능일까?"
categories: [Swift]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Rethrow

가끔 코드들을 보다보면 함수뒤에 `rethrows`라는 키워드가 달려있는 경우를 볼 수 있다.

처음에는 단순히 함수 내에서 throw메서드를 사용한 경우 do-catch문의 사용없이도 error catch가 일어난 경우 이 에러를 자동으로 throw한다는 뜻인가 싶었는데 이와는 전혀 다른 사용방법이 존재했다.

**요약**

> 함수 A가
>
> throw를 할지 안할지 모르는 함수를 인자로 받는 경우에
>
> rethrow 키워드를 A가 붙여줌으로써
>
> A를 호출할때 throw를 안하는 함수가 인자로 들어왔다면
>
> try를 쓰지 않아도 (do-catch 구문 없이도) 사용이 가능하게 해주는 것



```swift
extension String: Error { } // String값을 에러처럼 throw할 수 있도록 임시로 만든 extension

func authenticateBiometrically(_ user: String) throws -> Bool {
    throw "Failed"
}

func authenticateByPassword(_ user: String) -> Bool {
    return true
}

func authenticateUser(method: (String) throws -> Bool) rethrows {
    try method("twostraws")
    print("Success!")
}

authenticateUser(method: authenticateByPassword(_:)) 
// authenticatedByPassword(_:)는 throw를 하지 않으니 try 없이도 사용이 가능하다
```



authenticateUser 함수는 throw를 하는 함수를 인자로 받는다고 써놨음에도 

throwing을 하지 않는 authenticateByPassword를 인자로 받을 수 있다는 사실도 새롭게 알게 되었다.

(이는 authenticateUser에 rethrows가 아닌 throws를 붙여줘도 동일함)

<br/> 

throw하는 메서드를 받을수도 있고, rethrow를 하는 메서드를 받을수도 있는 경우에 유용하게 사용할 수 있을 것 같다.