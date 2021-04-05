---
layout: post
title: "Swift Lint 적용기"
image:
categories: Swift
tags: 
  - SwiftLint
  - CocoaPods
sitemap:
  changefreq: daily
  priority: 1.0
---

## Swift Lint 적용기

오늘은 Swift Lint와 코코아팟을 처음으로 적용해보았습니다.

코코아팟 설치, 사용법은 [이 링크](https://zeddios.tistory.com/25)에서 보실 수 있습니다 (고마워요 제드!)

<br/> 

간략하게 적용 과정을 요약하자면

<br/> 

`sudo gem install cocoapods`를 터미널에 입력하여 코코아팟 설치

터미널로 프로젝트 파일(`.xcodeproj`)이 위치한 경로로 이동하여 `pod init` 진행

pod init이 정상적으로 됐다면 Podfile이 생성되었을텐데 에디터를 통해 오픈 (`vim Podfile`)

Podfile의 target do 밑줄에 pod '원하는 라이브러리 이름' 을 적어주고 저장 (`pod 'SwiftLint'` 추가)

터미널에서 `pod install` 진행

<br/> 

위 작업이 완료되었다면 `.xcworkspace`라는 파일이 생겼을텐데 이것을 통해 프로젝트 파일을 오픈

BuildPhases의 추가 버튼 (`+`버튼)을 통해 `New Run Script Phase`를 진행

```
${PODS_ROOT}/SwiftLint/swiftlint
```

위의 코드를 추가하면 끝!



<br/> 

`.swiftlint.yml`이라는 파일을 만들어서 불필요한 규칙들을 disable 시켜줄 수 있습니다. (Xcode내에서 empty 파일로 추가할수도 있지만 오류가 잦아 터미널에서 추가하는 것이 좋아보입니다.)

규칙들은 아래의 링크를 통해 확인할 수 있습니다.

[Swift Lint 공식 깃허브](https://github.com/realm/SwiftLint)

[Swift Lint 공식문서](https://realm.github.io/SwiftLint/rule-directory.html)

이번 계산기 프로젝트에서 불필요하다 생각되는 규칙들은 다음과 같이 disable 시켜줬습니다.

```
disabled_rules:
- trailing_whitespace
- identifier_name
- line_length

```



<br/>



### 참고한 링크

[Swift Lint 공식 깃허브](https://github.com/realm/SwiftLint)

[Swift Lint 공식문서](https://realm.github.io/SwiftLint/rule-directory.html)

[왕초보를 위한 코코아팟 사용법 (Zedd)](https://zeddios.tistory.com/25)

[Swift Lint 사용법 (Zedd)](https://zeddios.tistory.com/447)

[Swift Lint 써보기 (야곰닷넷)](https://yagom.net/forums/topic/swift-lint-%EC%8D%A8%EB%B3%B4%EA%B8%B0/)

[Swift Lint (realm)](https://academy.realm.io/kr/posts/slug-jp-simard-swiftlint/)

