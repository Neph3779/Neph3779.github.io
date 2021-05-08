---
layout: post
title: "frame & bounds"
image:
categories: iOS
tags: 
  - frame
  - bound
  - origin
  - center
sitemap:
  changefreq: daily
  priority: 1.0
---

## frame

frame은 Superview 좌표계의 (0, 0)으로부터 얼마만큼 떨어져 있는지(위치) + view의 크기를 나타내는 값이다.

**Superview 좌표계의 (0, 0)은 Superview의 좌상단 꼭짓점이 아닐 수도 있다.** (Superview의 bounds가 변경된 경우)

이 문장이 frame과 bounds를 이해하는데 핵심이 된다.



## bounds

bounds는 view 자기 자신의 좌표계에서 원점(좌상단 꼭짓점)의 좌표값(origin의 좌표값) + view의 크기를 나타내는 값이다.



## frame과 bounds의 차이 - 그림을 통한 이해

다음 코드를 이해해보자

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509013223.png" alt="image-20210509013221679" style="zoom: 50%;" />



superview는 빨간색, subview는 파란색이다.

주목할 점은 superview의 bouns.origin 값을 설정하는 부분이다.

(-50, -50)으로 설정해준 것을 볼 수 있는데 이 말은 즉슨 빨간색 네모의 좌상단 꼭짓점의 좌표값이 (-50, -50)이라는 것이다.



이제 subview가 어디에 그려질지 상상해보자면

subview의 frame의 x값과 y값이 각각 0이므로 superview의 (0, 0)으로부터 (0, 0) 떨어진 위치에서부터 그려질 것이다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509013515.png" alt="simulator_screenshot_A7734F26-C68D-4995-9DCB-BE29FC951127" style="zoom: 25%;" />



superview의 관점에서 보았을 때 빨간색 네모의 좌상단 꼭짓점의 좌표는 (-50, -50)이며

파란색 네모의 좌상단 꼭짓점의 좌표가 (0, 0)에 해당하는 위치인 것이다. 



## ScrollView의 이해

위의 내용을 이해하면 한걸음 나아가서 scroll view의 동작원리에 대해 이해할 수 있다.

위의 빨간색, 파란색 네모 예제에서 `superview.bounds.origin`의 y값을 증가시키면 파란색 네모는 위쪽으로 올라간다.

아래의 그림은 `superview.bounds.origin`의 y값을 -50에서 -10으로 증가시켰을 때의 결과이다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509014113.png" alt="simulator_screenshot_8A7C995E-1E78-43E8-A7DD-F4CEB0F30B0D" style="zoom:25%;" />

빨간색 네모의 좌상단 꼭짓점의 좌표값은 (-50, -10)이 된 것이고

파란색 네모의 좌상단 꼭짓점은 superview의 (0, 0)이므로 위와 같은 결과가 나온 것이다.

이를 통해 ScrollView는 superview의 bounds.origin의 x값이나 y값을 스크롤하는 만큼 증가/감소시킨다는 것을 알 수 있다.

