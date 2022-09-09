---
layout: post
title: "UICollectionViewCompositionalLayout 정리 - 1"
categories: [iOS]
tags: 
  - compositional layout
  - collectionView
sitemap:
  changefreq: daily
  priority: 1.0
---

본 글은 [Getting Started with UICollectionViewCompositionalLayout](https://lickability.com/blog/getting-started-with-uicollectionviewcompositionallayout/)을 토대로 이해한 내용을 요약한 글입니다.

## 기존의 CollectionView 구현

- UICollectionViewFlowLayout을 사용
- UICollectionViewLayout을 subclassing

### Cell에 간격 주기

```swift
let flowLayout = UICollectionViewFlowLayout().then {
    $0.minimumInteritemSpacing = 10
    $0.minimumLineSpacing = 10
    $0.sectionInset = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
}
```

- flow layout의 속성값들을 조절하여 간격 조정 가능
- 이 작업은 flow layout의 delegate 메서드를 통해서도 구현할 수 있음

### Cell의 크기를 조절하기

```swift
extension ViewController: UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let width = collectionView.bounds.width
        let numberOfItemsPerRow: CGFloat = 3
        let spacing: CGFloat = flowLayout.minimumInteritemSpacing
        let availableWidth = width - spacing * (numberOfItemsPerRow + 1)
        let itemDimension = floor(availableWidth / numberOfItemsPerRow)
        return CGSize(width: itemDimension, height: itemDimension)
    }
}
```

- UICollectionViewDelegateFlowLayout을 채택하여 `sizeForItemAt` 메서드를 통해 Cell의 크기를 CGSize로 반환
- 본 글(2020년에 작성됨)에서는 화면 회전 이후에 bounds의 변경사항을 캐치하여 후속작업을 해줘야 원래 의도대로 셀 크기를 조절할 수 있다고 써있지만, 테스트 해본 결과 그런 작업을 해주지 않아도 정상적으로 작동함을 확인했음.



## Compositional Layout

### Group 개념의 등장

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220909145645.png" alt="Diagram of a compositional layout with items nested in groups within a section" style="zoom:50%;" />

- Compositional Layout에서는 group들의 모임이 Section, Group은 Item들의 모임
- Section과 item은 data source와 1대1 매칭을 이루지만 group은 그렇지 않음
  - section에 있는 item들의 layout을 describe하는 용도로 사용됨

### Compositional Layout 만들기

```swift
let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1/3),
                                      heightDimension: .fractionalHeight(1))
let item = NSCollectionLayoutItem(layoutSize: itemSize)

let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1),
                                       heightDimension: .fractionalWidth(1/3))
let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])

let section = NSCollectionLayoutSection(group: group)
let compositionalLayout = UICollectionViewCompositionalLayout(section: section)
```

- NSCollectionLayoutSize에 NSCollectionLayoutWidth, NSCollecitonLayoutHeight 값을 주어서 크기를 자신의 상위 계층에 대한 상대적인 비율로 설정
  - 해당 예제에서 width 1이 의미하는 바는 가로 길이의 1(100%)를 쓰겠다는 것
  - height 1/3은 세로 길이의 1/3을 쓰겠다는 것
- NSCollectionLayoutGroup의 class 메서드를 활용해 group을 생성할 수 있음
- 낮은 단계부터 설명하자면 item 제작 -> group 제작 -> section 제작 -> layout 제작



위의 예제에서는 비율로 size를 지정해주었지만

- NSCollectionLayoutDimension.absolute(_:) 메서드로 구체적인 수치를 주어 제작할수도 있고
- NSCollectionLayoutDimension.estimated(_:)를 통해 self-sizing에도 대비할 수 있다.



### Compositional Layout에서 Margin, Spacing 주는 법

```swift
let item = NSCollectionLayoutItem(layoutSize: itemSize)
item.contentInsets = NSDirectionalEdgeInsetsNSDirectionalEdgeInsets(top: 5, leading: 5, bottom: 5, trailing: 5)

let section = NSCollectionLayoutSection(group: group)
section.contentInsets = NSDirectionalEdgeInsetsNSDirectionalEdgeInsets(top: 5, leading: 5, bottom: 5, trailing: 5)
```

> 이때 주의할 점은 Flow Layout에서의 spacing과 다르게 contentInset은 각각의 item, section마다 적용되는 것이므로 의도한 spacing의 절반에 해당하는 값을 주어야 의도만큼 띄워짐

- 이렇게 간격을 띄워놔도 "비율"로 몇개의 item이 section에 들어갈건지 설정했기 때문에 추가적인 수식 계산 필요없음



### 이번글에서 살펴본 Compostional Layout의 장점

- sizing logic의 단순화 (flow layout delegate에서의 frame이나 bounds를 체크하여 숫자로 데이터를 다루던 부분이 없어짐)