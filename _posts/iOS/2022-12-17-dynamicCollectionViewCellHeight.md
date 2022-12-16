---
layout: post
title: "UICollectionViewCell dynamic height 적용"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---



본 글에서는 DataSource를 구현하기 위해 UICollectionViewDataSource 프로토콜을 사용했으며

Layout을 구현하기 위해 UICollectionViewFlowLayout과 그 delegate을 사용하였습니다.



## 상황

CollectionViewCell의 높이를 내부 contents의 길이(양)에 따라 딱 맞도록 조절하고 싶었다.



## 해결방법

collectionView(_:layout:sizeForItemAt:)를 통해 cell의 size를 전달할 수 있고

collectionView(_:cellForItemAt:)을 통해 cell의 contents를 configure할 수 있지만

collectionView(\_:layout:sizeForItemAt:)이 collectionView(\_:cellForItemAt:)보다 먼저 호출되는 것이 문제였다.

이를 해결하기 위해 collectionView(_:layout:sizeForItemAt:)에서 cell의 content를 채워준 뒤

이를 통해 height를 구해주었다. (실제로 cell의 content를 채우는 시점은 collectionView(_:cellForItemAt:)의 호출 시점)

"딱 맞는" size를 구하기 위해 systemLayoutSizeFitting(_:) 메서드를 사용했다. (UIView의 instance 메서드)



## 코드

```swift
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let width = collectionView.frame.width - 2 * Constraints.outerCollectionViewInset
        guard let height = Section(rawValue: indexPath.section)?.estimatedCellHeight else { return .zero }

        if Section(rawValue: indexPath.section) == .detailReviews {
            let dummyCellForCalculateheight = DetailReviewCell(frame: CGRect(origin: CGPoint(x: 0, y: 0),
                                                      size: CGSize(width: width, height: height)))
            dummyCellForCalculateheight.setUpContents(detailReview: detailReviewViewModel.detailReviews[indexPath.row])
            let heightThatFits = dummyCellForCalculateheight.systemLayoutSizeFitting(CGSize(width: width, height: height)).height
            return CGSize(width: width, height: heightThatFits)
        }

        return CGSize(width: width, height: height)
    }
```



나의 경우에는 특정 section의 cell만 dynamic height를 적용하면 됐으므로 이에 대한 분기처리가 이루어졌다.

dummyCellForCalculateHeight를 통해 넉넉한 frame size의 cell을 제작한 뒤 (estimatedCellHeight는 미리 넉넉하게 잡아둔 constant)

cell에 contents를 적용하고나서 systemLayoutSizeFitting 메서드를 통해 딱 맞는 height를 계산해주고

이를 적용시켜줬다.



주의할 점은 setUpContents() 메서드 내부에서 layoutIfNeeded()나 layout() 메서드를 호출해줘야 한다.





**참고한 글**

https://corykim0829.github.io/ios/UICollectionViewCell-dynamic-height/#