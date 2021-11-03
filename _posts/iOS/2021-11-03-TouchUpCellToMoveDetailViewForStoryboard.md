---
layout: post
title: "Storyboard에서 TableView Cell 터치시 DetailView로 이동하는 방법"
categories: [iOS]
tags: 
  - TableViewCell
  - Touch cell to move detailView
sitemap:
  changefreq: daily
  priority: 1.0
---

## Storyboard + TableView + Segue 활용하기

스토리보드로 작업 시 Segue를 버튼으로 연결하여 다른 ViewController를 띄우는 것과 다르게 TableView Cell 터치시 DetailView로 이동하는 작업은 단순 Segue 연결만으로 해결할 수 없었다. 

didSelectRowAt에서 간단히 push를 통해 DetailView를 띄워줄 수도 있지만 스토리보드의 장점인 flow의 가시성을 활용하기 위해 Segue로 연결하면 좋겠다 생각이 들어 방법을 찾아보았고, 이를 정리해두는 글이다.

List와 Detail을 연결하여 Segue를 만든다 (ViewController끼리 연결해도 되고, Cell과 DetailViewController를 연결해도 된다. 동작은 동일.)

그 뒤에 코드로 와서

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        performSegue(withIdentifier: "ShowDetail", sender: nil)
    }
```

다음과 같이 cell이 선택됐을때 performSegue를 호출해주면 된다. (데이터를 넘겨야한다면 이 지점에서 진행)

Segue의 identifier는 별도로 지정을 해주어야하며 Xcode의 Inspector 영역에서 지정해줄 수 있다.

