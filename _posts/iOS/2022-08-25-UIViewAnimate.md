---
layout: post
title: "UIView animation, animateKeyframes 정리"
categories: [iOS]
tags: 
  - animation
  - keyframe
sitemap:
  changefreq: daily
  priority: 1.0
---



## UIview.animate, UIView.transition 메서드

### animate(withDuration: delay:usingSpringWithDamping:initialSpringVelocity options: animations:completion:)

- 애니메이션이 진행될 시간값인 duration
- completion block은 애니메이션이 끝난 뒤에 실행될 요소
- 이동, 크기 조절, 회전, fading 등 거의 모든 애니메이션 작업을 진행할 수 있음
- 애니메이션 옵션으로는 `curveEaseInOut`과 `transitionNone` 을 사용함
- usingSpringWithDamping은 말그대로 view가 스프링처럼 탄력을 가지고 움직이게 해주는 옵션
  - 이 값을 크게 해줄수록 스프링 효과가 적음 (0~1)

- options에는
  - 애니메이션을 진행할 view의 subview들이 같이 애니메이팅 되도록 만들어주는 `layoutSubviews`
  - 애니메이션 진행 도중 user interaction을 가능하게 해주는 `allowUserInteraction` (이 옵션이 켜져있지 않다면 기본적으로 애니메이션 도중에는 유저 상호작용이 불가)
  - 이미 애니메이션이 진행중일때 추가적인 애니메이션을 현재 상태부터 실행할 수 있도록 해주는 `beginFromCurrentState`
  - 애니메이션을 무한반복하는 `repeat`
  - 애니메이션이 실행된 후 반대 방향으로 다시 애니메이션을 실행하는 `autoreverse` (1->10 이었다면 애니메이션 종료시에 10->1로의 애니메이션이 진행됨)
  - 애니메이션의 처음, 끝 등의 속도를 설정할 수 있는 `curve`옵션들 
  - 등 여러가지가 존재
  - transition관련 옵션들도 존재하지만 이 옵션들은 UIView.transition에 넣어야 동작함



### transition(with:duration:options:animations:completion:)

- animate 메서드와 큰 틀에서 비슷함
- with 파라미터에는 UIView가 들어가는데 어떤 view를 transition할지를 받아옴
- animate 메서드와 마찬가지로 animations 블록에 transition을 진행하면서 진행할 애니메이션을 추가할 수 있음
  - transition을 통해 뒤집기를 진행하면서 동시에 이동도 가능함



animate, transition 옵션들과 실제 동작을 보고싶다면

[Swift UIView lovely animation and transition | by Myrick Chow | ITNEXT](https://itnext.io/swift-uiview-lovely-animation-and-transition-d34bd623391f)

이곳으로..



## animateKeyframes, addKeyframe

### animateKeyframes(withDuration:delay:options:animations:completion)

- duration, delay 는 위에서 봤던 것들과 같음
- animations 블록에 addKeyframe 메서드를 통해 keyframe들을 추가할 수 있음



### addKeyframe(withRelativeStartTime:relativeDuration:animations)

- 기존의 UIView.animate를 통해 1번 애니메이션이 끝난 뒤 2번을 실행하고 또 3번을 실행하였다면 completion 블록이 중첩되어 쌓이게 됨
- animateKeyframes의 animation 블록에 addKeyframe 메서드를 넣어줌으로써 이러한 것을 해결 가능
- relative start time은 언제 시작할지를 0~1 사이의 값을 줘서 결정
  - 0.5를 주었다면 설정한 duration 중 절반이 지나간 시점에서 시작됨
- relative duration은 animateKeyframes에서 준 duration값의 몇퍼센트를 지금 추가할 keyframe이 가져갈지를 결정함
  - 0~1 사이의 값을 넣어주면 그만큼의 지분을 가지고 애니메이션을 실행
- 공식문서에는 언급이 없지만 relative duration을 만족할 수 없다면 (relative start time이 0.99인데 relative duration은 1인 케이스 등) relative start time이 우선적으로 적용됨
- animations 블록에는 원래 UIView.animate에서 해줬던 것처럼 진행하면 됨
- completion 블록의 중첩 없이 여러 애니메이션을 간결하게 처리할 수 있음



## 마무리

이번 공부를 통해서 애니메이션를 수행하는 두가지 방법에 대해 알게 되었습니다.

다음번에는 animations 블록에 들어갈 수 있는 여러 요소들 (frame의 변경, auto layout의 변경, transform을 활용한 rotation 등)을 다루어 보겠습니다.