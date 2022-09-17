---
layout: post
title: "Diffable Datasource"
categories: [iOS]
tags: 
  - diffableDatasource
sitemap:
  changefreq: daily
  priority: 1.0
---

## 기존 datasource의 문제점

- index 기반의 배열접근 작업이기 때문에 잘못접근시 시스템 에러 발생
- datasource를 업데이트 하고 해당 위치의 cell을 "알맞게" 업데이트 해주어야하는 번거로움 존재
  - 만약 변경된 datasource와 맞지 않는 view 업데이트 작업 진행시 어김없이 에러 발생
  - cell 이동, 추가, 삭제 등 작업을 여러개 실행하려면 perfromBatchUpdate 등을 통해 작업해야하는 번거로움 있음
  - Data의 "상태"와 UI의 "상태"가 반드시 일치하도록 해야하는데 중앙화가 안되어 있어서 따로따로 작업해야함



## NSDiffableDatasource 장점

- snapshot을 찍어서 apply해놓으면 그 snapshot 덕분에 배열에 index로 접근할 필요가 없음
  - cellProvider에서 itemIdentifier (우리가 사용하는 cell)을 어떠한 배열에도 접근하지 않고 사용할 수 있는 이유가 이 때문
- 변경사항이 있을때 새로운 snapshot을 apply해주면 UI는 자동으로 반영됨
  - 기존 datasource는 datasource에 대한 업데이트, UI에 대한 업데이트가 모두 따로였고 심지어 프로그래머가 UI를 "알맞게" 업데이트 해줘야 했지만 diffableDatasource는 그러한 위험부담, 귀찮음 없음
- Section의 enum화 관리가 강제됨
  - 단점으로 보일수도 있지만 index를 통해 관리하던 것은 단지 귀찮아서였으므로 기존의 방법이 더 좋다고 평가하기 어려움



## 기존에 사용하던 index 기반 코드들은?

- 그대로 사용할 수 있음

  - ```swift
    func collectionView(_ collectionView: UICollectionView,
                        didSelectItemAt indexPath: IndexPath) {
                        if let identifier = diffableDataSource.itemIdentifier(for: indexPath)
                        }
    ```

    위와같이 diffableDatasource는 indexPath를 안다면 itemIdentifier를 사용할 수 있음

  - 여기서 itemIdentifier는 그냥 cell 그 자체를 의미한다고 보면 됨



## NSDiffableDatasource 한계점

- datasource 관련 코드를 직접 분할해야 함
  - 기존에는 datasourceDelegate의 extension을 통해 제공하던 틀이 존재하여 적절한 메서드를 통해 작업을 진행했지만, 이제는 작업을 어떻게 하든 프로그래머의 자유가 됨
  - 이는 가독성의 저하로 이어질 우려가 있음
- enum을 통한 section 관리시에 rawValue를 사용하지 않고서는 마땅한 방법이 없음
  - index 값을 통한 관리에서 enum을 하나 만들어 section을 모아 관리하는 것까지는 좋았으나 결국 rawValue를 통해 section의 case에 접근하는것이 최선임. 추가적인 API를 통해 section의 관리도 지원해줬으면 하는 바람..



## 리팩토링 내역

기존 살까말까의 homeView에서 사용되던 datasource는 diffableDatasource로,

layout은 tableView내부에 2개의 CollectionView를 넣던 방식에서 compositionalLayout으로 리팩토링을 진행하였음.

- [[Refactor\] Compositional layout 적용 by Neph3779 · Pull Request #2 · Neph3779/BuyOrNot (github.com)](https://github.com/Neph3779/BuyOrNot/pull/2)
- [[Refactor\] DiffableDatasource 적용 by Neph3779 · Pull Request #3 · Neph3779/BuyOrNot (github.com)](https://github.com/Neph3779/BuyOrNot/pull/3)