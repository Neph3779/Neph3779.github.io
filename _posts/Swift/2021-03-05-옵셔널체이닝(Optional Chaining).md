---
layout: post
title: "옵셔널체이닝(Optional Chaining) 알아보기 (feat. Flow chart)"
image: 
categories: Swift
tags:
  - Swift
  - Flow chart
---

### 안녕하세요! Neph🌱입니다!! 😆😆

오늘은 Swift의 옵셔널체이닝에 대해 알아볼거에요

`Optional`과 `Optional Binding`에 대해 모르시는 분들은 먼저 해당 개념을 익히신 뒤 글을 읽어주시면 감사하겠습니다 🥲

<br/>

자 그럼 Optional Chaining! 뭔가 이름만 들어도 `Chain`, 연쇄적으로 어떤 것을 처리할 것이라는 느낌이 들지 않으신가요? 

<br/>

`Optional Chaining`은 `Optional Binding`과 마찬가지로 Optional에 속해 있는 nil일지도 모르는 값을 꺼내오는 방법 중 하나입니다. 

우선 익숙하게 보셨을 guard let 구문을 통한 Optional Binding 예제를 살펴보실까요?

아래의 코드는 게임 내에서 유저가 입력한 값을 검사하는 `checkedUserInput` 함수입니다.

어떻게 Optional을 벗기고 값을 추출해줬는지 생각해보면서 코드를 읽어봅시다!

```swift

func checkedUserInput(_ optionalStringUserInput: String?) throws -> Int{
  
	guard let stringUserInput = optionalStringUserInput else {
		throw GameError.invalidInput
	}
  
	guard let integerUserInput = Int(stringUserInput) else {
		throw GameError.invalidInput
	}
  
	return integerUserInput
}
```

<br/>

첫 guard let 구문은 함수를 통해 받아온 `Optional<String>` 에서 `String`을 추출하여 `stringUserInput`이라는 변수에 저장하는 과정입니다!

만약 전달된 `optionalStringUserInput`을 통해 nil이 전달되었다면 두번째 guard let 구문은 실행되지 않은 채 함수가 종료되겠죠?

두번째 guard let 구문은 `stringUserInput`에 들어있는 `String`타입의 변수를 `Int`로 변환하는 과정입니다. 이 과정에서 역시`stringUserInput`에 들어있는 값이 `Int`로 변환될 수 없다면 역시 throw와 함께 함수가 종료될거에요!

<br/>

이렇게 두개의 guard let 구문을 사용하여 `Optional<String>`과 `Optional<Int>`에서 값을 추출해줬네요

🧐 흠.. 그럼 여러개의 Optional type의 변수에서 값을 추출하려면 매번 guard let 혹은 if let을 사용해줘야 할까요?

<br/>

그렇지 않습니다!

이런 어려움을 해결할 수 있는 좋은 방법이 바로 Optional Chaining 입니다!

<br/>

Optional Chaining을 이용하면 여러개의 Optional 변수의 값을 연쇄적으로 해제합니다

만약 중간에 nil값으로 인해 값을 추출할 수 없다면 해당 구문은 실행되지 않고 멈추게 됩니다.

아래 코드를 보면서 이해해봅시다!

<br/>

```swift
func checkedUserInput(_ optionalStringUserInput: String?) throws -> Int {
	guard let stringUserInput = optionalStringUserInput,
  	let integerUserInput = Int(stringUserInput)
	else {
		throw GameError.invalidInput
	}
	return integerUserInput
}
```

<br/>

두개로 분리되어있던 guard let 구문을 하나로 합쳤습니다!

두개의 변수는 `,`를 통해 구분해주었네요! (`&&`을 사용해줘도 동일하게 동작합니다)

어떤가요? 이렇게 여러개의 Optional 값을 한번에 처리해주니 편리하지 않나요?

아직 로직이 이해가 잘 가지 않으실 분들을 위해 Flow chart를 준비했습니다!

<p align="center"><img src="/assets/images/posts/OptionalChainingFlowChart.png" alt="OptionalChainingFlowChart" style="zoom:50%;" /></p>

함수가 시작하며 받아온 `optionalStringUserInput`이 nil이라면 throw GameError.invalidInput과 함께 함수가 종료됩니다.

만약 nil이 아니라면 stringUserInput에 `optionalStringUserInput`을 할당해주고(값을 저장해주고) `Int`로 변환가능한지에 대해서 체크합니다.

이 과정에서 `Int`로 변환할 수 없다면 해당 `Int(stringUserInput)`은 nil이 되며 위에서와 마찬가지로 throw GameError.invalidInput과 함께 함수는 종료됩니다.

<br/>

Optional Chaining에 대해 알아봤는데요 

Optional Chaining을 단순히 guard let 구문을 줄이기 위해서만 사용하는것이 아닙니다.

첫번째 조건문에서 nil값이 발견되면 함수가 바로 종료되는 것이 핵심인데요 이를 함수의 빠른종료(Early Exit)라 합니다.

로직이 길어지면 길어질수록 빠른 종료가 빛을 보게 될거에요!

이 부분에 대해서는 다음에 더 자세히 다루어보도록 할게요!

<br/>

>### 피드백 언제나 환영합니다!