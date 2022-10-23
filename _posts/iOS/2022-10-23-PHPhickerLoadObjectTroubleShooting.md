---
layout: post
title: "PHPicker itemProvider loadObject 트러블슈팅"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## loadObject

```swift
let images = results
            .map { $0.itemProvider }
            .map { item in
                let name = item.suggestedName
                var itemImage: UIImage?
                var itemData: Data?
                if item.canLoadObject(ofClass: UIImage.self) {
                    item.loadObject(ofClass: UIImage.self) { image, _ in
                        itemImage = image as? UIImage 
                    }
                }
                itemData = itemImage?.pngData()
                return ItemManagingViewModel.PickedImage(name: name, image: itemImage, data: itemData)
            }
```

이러한 로직으로 데이터를 저장하려 했는데

계속 image에 nil이 들어있길래 뭐가 문젠가 했더니

![image-20221023173733373](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221023173733.png)

비동기적으로 로딩하는것이고 뒷부분은 completionHandler여서

저렇게 map가지고 한번에 인스턴스를 만들 수 없던 것이었다

