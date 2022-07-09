---
layout: post
title: "[iOS] query에 띄어쓰기가 있을때 처리하는 방법"
image:
categories: iOS
tags: 
  - percentEncodedQueryItems
  - URLComponents
sitemap:
  changefreq: daily
  priority: 1.0

---



보내야 하는 Query에 띄어쓰기가 있는 경우 이를 해석하지 못해 오류가 발생하곤 합니다.

이때는 percentEncodedQuery로 바꿔주어야 합니다. (인터넷 링크를 복사해 붙여넣으면 한글은 영어로, 띄어쓰기는 %로 바뀌는 것을 떠올리면 됩니다.)



```swift
final class YoutubeCrawler {
    private var urlComponents: URLComponents = {
        var urlComponents = URLComponents()
        urlComponents.scheme = "https"
        urlComponents.host = "youtube.com"
        urlComponents.path = "/results"
        return urlComponents
    }()
    
     func fetchYoutubeReviews(query: String, completion: @escaping ([String?]) -> Void) {
        let searchQuery = query.split(separator: " ").joined(separator: "+")
        let percentEncodedQuery = searchQuery
       				.addingPercentEncoding(withAllowedCharacters: .urlQueryAllowed)
        urlComponents.percentEncodedQueryItems = [URLQueryItem(name: "search_query",
                                                               value: percentEncodedQuery)]
        guard let url = urlComponents.url else { return }
       //...
     }
  //...
}
```

먼저 URLComponents를 통해 url의 요소들을 정의해주고

query를 기존에 urlComponents.queryItems가 아닌 urlComponents.percentEncodedQueryItems에

query를 percentEncoding하여 담아줍니다.

여기서 `addingPercentEncoding(withAllowedCharacters: CharacterSet)`은 String의 인스턴스 메서드입니다.

