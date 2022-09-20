---
layout: post
title: "cell highlight와 select 차이"
categories: [iOS]
tags: 
  - highlight
  - select
sitemap:
  changefreq: daily
  priority: 1.0
---

## Highlight vs Select

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220919221525.png" alt="A diagram depicting the steps that occur when tapping an unselected cell. The first image shows the user’s right index finger touching a dark, shaded rectangle representing a collection view cell on a phone screen. Below the image are current property values of the cell, listed as isHighlighted: true and isSelected: false. The second image depicts the user lifting their finger and moving their hand to the right of the rectangle, now shown shaded in a lighter color, on the phone screen. Below, the listed properties isHighlighted and isSelected are both false. The third image depicts the user’s hand in the lifted position above the rectangle on the phone screen, with the collection view presenting the rectangle in a third and different shade. Below, the property isHighlighted is false and isSelected is true." style="zoom:67%;" />



Highlight의 경우 셀을 터치하고 있거나 스와이프하기만 해도 동작

반면 select는 터치하고 그 자리에서 손을 떼는 (일반적인 tap 동작) 동작에만 해당



| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220919221749.gif" alt="highlight" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220919221757.gif" alt="select" style="zoom:50%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|             Highlight시에 cell에 변화를 준 경우              |               Select시에 cell에 변화를 준 경우               |

