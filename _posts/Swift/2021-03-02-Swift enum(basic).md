---
layout: post
title: "Swift enum에 대해 알아보자! (기본편)"
image:
categories: Swift
tags:
  - Swift
  - 열거형
  - enum
---


#### 안녕하세요 Neph🌱입니다!! 😆😆 

##### 오늘은 Swift의 자료형 중 하나인 enum(열거형)에 대해 알아볼게요  

<br/>

> 오늘도 어김없이 야곰의 스위프트 프로그래밍 3판과 함께합니다 🙂  

<br/><br/>  

##### 백문이 불여일견! 우선 enum을 사용한 코드를 보시죠
<br/>
```swift
enum Season {
	case spring
	case summer
	case fall
	case winter
}
<br/>
enum Season {
  case spring, summer, fall, winter
} // 위의 코드와 동일하게 동작합니다.
```
<br/><br/>


enum은 연관된 항목을 묶어서 표현할 수 있는 타입입니다!

`Season` 이라는 enum type을 생성해준 것이죠  
<br/><br/>


##### 🥸 오오.. 내가 임의로 변수의 타입을 생성할 수 있군요! Struct와도 유사해보이는데 enum은 언제 사용하는게 좋을까요?  
<br/><br/>


##### enum type은 다음과 같은 경우에 요긴하게 사용할 수 있습니다

> - 제한된 선택지를 주고 싶을 때
> - 정해진 값 외에는 입력받고 싶지 않을 때
> - 예상된 입력 값이 한정되어 있을 때  

  <br/><br/>

##### 계절은 4계절이 있으니 enum type을 통해 Season type을 만들어주면 편리하겠죠? 😆  

<br/>

---

<br/>

자 그럼 이런 경우에 enum을 사용하면 어떨까요?

```swift
enum Month {
	case january // 1
  case february // 2
  case march // 3
  case april // 4
  case may // 5
  case june // 6
  case july // 7
  case august // 8
  case september // 9
  case october // 10
  case november // 11
  case december // 12
}
```

<br/><br/>

12개의 달을 각각의 case로 만들어줬고 주석에는 해당하는 달의 숫자를 적어줬어요!

음.. 각각의 case에 맞는 값(주석으로 처리해준 값)을 case에 저장해줄수는 없을까요?

<br/>

Swift에서는 이런 점을 해결하기 위해 case마다 원시 값(Raw Value)를 가질 수 있습니다!

코드부터 보시죠

```swift
enum Month: Int {
	case january = 1
  case february = 2
  case march = 3
  case april = 4
  case may = 5
  case june = 6
  case july = 7
  case august = 8
  case september = 9
  case october = 10
  case november = 11
  case december = 12
}
```

  <br/><br/>

이렇게 case마다 고유의 값(원시 값)들을 배정해줄 수 있습니다! 

그러면 이제 원시 값을 출력해볼까요?

<br/><br/>

```swift
let familyMonth: Month = .may
// var familyMonth = Month.may 와 동일하게 작동합니다.

print("가정의 달은 \(familyMonth.rawValue)월 입니다.")
// 출력 : 가정의 달은 5월 입니다.
print("가정의 달은 \(familyMonth)월 입니다.")
// 출력 : 가정의 달은 may월 입니다.
```
<br/><br/>


첫번째 print 문장과 두번째 print 문장의 차이가 보이시나요?

첫번째 print문장과 같이 `.rawValue` 를 통해 접근해줘야 원시 값을 출력할 수 있어요!

두번째 print문장처럼 적는다면 case의 이름인 `may` 가  출력되니 주의해주세요 😫😫

<br/>

##### 🧐 rawValue를 통해 memberOfNephCorp 변수를 바로 초기화할수는 없을까요??  

가능합니다! 아래의 코드를 보시죠

```swift
let scienceMonth = Month(rawValue: 4)
// 4라는 rawValue가 존재하므로 april이 할당됨
let weirdMonth = memberOfNephCorp(rawValue: 13)
// 13이라는 rawValue가 없으므로 nil이 할당됨
```

<br/>

주석에 적어놓은것처럼 `rawValue` 를 통해 검색했을 때 찾을 수 없다면 nil이 할당됩니다. (dictionary 자료형과 유사하네요😉) 

<br/>

---

<br/>

> #### 만약 각각의 case가 독특한 점들이 있다면 이를 어떻게 저장해줄 수 있을까요?  

바로 **연관값**을 통해 저장해줄 수 있습니다

```swift
enum ItalianFood {
	case pizza(dough: String, topping: String)
	case pasta(diameter: Int, boilingTime: Int, shape: ShapeOfPasta)
	case lasagna(fillings: String)
}
```

<br/>

##### 이렇게 각각의 case가 자신과 연관된 값을 가질 수 있습니다!  

<br/>

---

  <br/>

##### 오늘 배운 내용만으로도 충분히 강력한 enum type이지만 이보다 더 다양한 기능들을 구현할 수 있습니다


###### 이 부분에 대해서는 Swift enum에 대해 알아보자! (고급편)에서 만나봐요~ 👋👋

<br/><br/>

> # 피드백은 언제나 환영합니다!



---

#### 수정 이력

- 2021/03/06: rawValue의 활용사례가 적절하지 않은 것 같다는 yagom의 피드백을 수용하여 [이 글](https://devxoul.gitbooks.io/ios-with-swift-in-40-hours/content/Chapter-3/enums.html)을 참고하여 활용사례를 수정함.



