---
layout: post
title: "[Core] HTTPHeaders.swift 분석"
image:
categories: [iOS]
tags: 
  - Alamofire
  - HTTPHeaders
sitemap:
  changefreq: daily
  priority: 1.0
---

## HTTPHeaders

### HTTPHeaders는 struct

HTTPHeaders는 struct 타입이다. 내부에서 reference 타입을 다루지 않기 때문에 ARC로 인한 성능저하를 우려하지 않아도 된다.



### HTTP header를 담고 있는 배열이 있다.

```swift
private var headers: [HTTPHeader] = []
```

headers라는 private으로 선언된 HTTPHeader의 배열이 존재한다.

HTTPHeader는 Alamofire에서 제공하는 struct이다.



### 세 종류의 init

```swift
public init() {}

public init(_ headers: [HTTPHeader]) {
    self.init()
    headers.forEach { update($0) }
}

public init(_ dictionary: [String: String]) {
    self.init()
    dictionary.forEach { update(HTTPHeader(name: $0.key, value: $0.value)) }
}
```

빈 instance를 생성하는 이니셜라이저, HTTPHeader 배열을 받아 headers 초기화하는 이니셜라이저, header를 key와 value가  String 딕셔너리를 받아 headers를 초기화하는 이니셜라이저가 있다.



### header의 추가, 삭제, 정렬

```swift
public mutating func add(name: String, value: String) {
    update(HTTPHeader(name: name, value: value))
}

public mutating func add(_ header: HTTPHeader) {
    update(header)
}

public mutating func update(name: String, value: String) {
    update(HTTPHeader(name: name, value: value))
}

public mutating func update(_ header: HTTPHeader) {
    guard let index = headers.index(of: header.name) else {
        headers.append(header)
        return
    }
    headers.replaceSubrange(index...index, with: [header])
}

public mutating func remove(name: String) {
    guard let index = headers.index(of: name) else { return }
    headers.remove(at: index)
}

public mutating func sort() {
    headers.sort { $0.name.lowercased() < $1.name.lowercased() }
}

public func sorted() -> HTTPHeaders {
    var headers = self
    headers.sort()
    return headers
}
```

add, update를 통해 기본적인 header의 추가가 가능하다. HTTPHeader 타입을 통해서 추가할수도 있고 key value String 쌍을 가지고도 가능하다. sort는 header를 알파벳 소문자순으로 정렬해주는 기능을 한다.



### header의 value 값

headers에 담긴 원소는 두가지 방법을 통해 접근할 수 있다. 

1. `public func value(for name: String) -> String?`을 이용해 key값을 String으로 넣어주면 value를 String?으로 받아볼 수 있다.
2. `public subscript(_ name: String) -> String?` subscript가 구현되어 있으므로 대괄호를 통해 딕셔너리에 접근하듯 접근할 수 있다.

추가로 dictionary라는 연산프로퍼티가 존재하며 headers를 통해 Dictionary 타입을 만들어 리턴한다.

### HTTPHeaders가 추가로 conform하는 프로토콜들

- ExpressibleByDictionaryLiteral
  HTTPHeaders type의 instance는 그 자체로 dictionary로 취급될 수 있다. 

- ExpressibleByArrayLiteral
  HTTPHeaders type의 instance는 그 자체로 arrary로 취급될 수 있다. 

- Sequence

- Collection
  Sequence, Collection 프로토콜을 conform하고 있다. Collection conform을 통해 subscript가 구현되어 있으므로 index를 통해 접근할 수 있으며 startIndex, endIndex, Index(after:)를 사용할 수 있다.

- CustomStringConvertible
  설명보다 코드를 보는게 이해가 더 빠를 것 같아 코드를 첨부

  ```swift
  public var description: String {
      headers.map { $0.description }
          .joined(separator: "\n")
  }
  ```



### 추가 extension들

파일의 하단에 보면 `URLRequest`, `HTTPURLResponse`, `URLSessionConfiguration`의 extension을 통해 headers라는 `HTTPHeaders` type의 변수를 연산 프로퍼티로 가지게 했음을 알 수 있다. 이를 통해 해당 타입의 프로퍼티로 headers를 사용할 수 있다.



이 글에 설명된 것 외에도 HTTPHeader struct, type property인 default도 구현되어있다. 
[https://github.com/Alamofire/Alamofire/blob/master/Source/HTTPHeaders.swift](https://github.com/Alamofire/Alamofire/blob/master/Source/HTTPHeaders.swift)

