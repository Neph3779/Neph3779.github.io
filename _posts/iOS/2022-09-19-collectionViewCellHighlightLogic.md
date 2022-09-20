---
layout: post
title: "collectionView cell 눌렀을 때 cell 색상 변하게 하기"
categories: [iOS]
tags: 
  - collectionView
  - cell highlight
sitemap:
  changefreq: daily
  priority: 1.0
---

## Cell을 눌렀을때 유저가 눌렀다는 인식을 받도록 하기

table view의 경우 cell을 누르면 cell이 회색으로 변하며 "눌렀다"라는 인상을 주기 쉽습니다.

하지만 collectionView cell은 마땅한 방법들이 안보여서 이것저것 시도해보고 제일 적당한 방법을 찾았기에 소개하려 합니다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220919170515.gif" alt="cellTapTestgif" style="zoom:50%;" />



### 방법 1 (채택됨)

cell을 모두 덮는 maskView를 하나 만들어 maskView의 backgroundColor로는 darkGray를,

opacity는 0.3을 주어서 반투명한 회색 층을 하나 만드는 방법입니다.

이 maskView의 isHidden 속성을 껐다 켰다 해주면서 cell을 눌렀다는 느낌을 받도록 할 수 있었습니다.

**실패한 코드**

```swift
if let cell = collectionView.cellForItem(at: indexPath) as? CategoryCell {
	UIView.animate(withDuration: 0.1, delay: 0, animations: {
		cell.selectMask.isHidden = false
	}, completion: { _ in
	  cell.selectMask.isHidden = true
	  self.navigationController?.pushViewController(RankViewController(category: category),
                                                  animated: true)
	})
}
```

처음에는 UIView의 animate 메서드나 animateKeyframes 메서드를 활용해

`cell 클릭` -> `깜빡이는 애니메이션 실행과 동시에 다음화면 진입` 과 같은 프로세스를 노렸으나, 

completion 블록에서 화면전환 코드를 넣어도 해결이 되지 않았기 때문에 선택한 방법은 아래의 방법입니다.

**성공한 코드**

```swift
if let cell = collectionView.cellForItem(at: indexPath) as? CategoryCell {
    homeViewModel.highlightedIndexPathForCategoryCell = indexPath
    cell.selectMask.isHidden = false
} else if let cell = collectionView.cellForItem(at: indexPath) as? RecommendProductCell {
    homeViewModel.highlightedIndexPathForRecommendProductCell = indexPath
    cell.selectMask.isHidden = false
}
```

누른 cell의 maksView의 isHidden을 animation 과정에서 조절하는 것이 아닌, 누른 시점에 maskView의 isHidden을 false로 바꿔준 뒤,  indexPath를 viewModel로 하여금 들고있게 하였습니다.

그리고 다시 cell이 있던 view로 돌아올 때, viewModel이 들고 있던 indexPath를 활용해 maskView의 isHidden을 true로 바꿔주었습니다.





### 방법 2 (고려해볼만 함)

animation 블록에서 maskView의 isHidden을 껐다 켰다 하는 것에는 실패했지만, cell의 opacity를 조절하는 것에는 성공했습니다. 

다만 이땐 깜빡이는 animation이 끝난 뒤, completion블록에서 화면전환이 이루어지기에 불필요한 딜레이가 필연적으로 발생합니다.

제 기준에서는 용납될만한 선의 딜레이였던지라 고려해볼만 한 방법에 넣어봤습니다.

```swift
if let cell = collectionView.cellForItem(at: indexPath) as? CategoryCell {
	UIView.animate(withDuration: 0.1, delay: 0, animations: {
		cell.layer.opacity = 0.5
	}, completion: { _ in
	cell.layer.opacity = 1
	self.navigationController?.pushViewController(RankViewController(category: category),
                                                animated: true)
	})
}
```

