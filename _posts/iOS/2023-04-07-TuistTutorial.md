---
layout: post
title: "Module, Library, Framework, Package, Product... 뭐가 다른걸까"
image:
categories: [iOS]
tags: 
  - SPM
  - SwiftPackageMaanger
  - Module
  - Package
  - Library
sitemap:
  changefreq: daily
  priority: 1.0
---



## Bundle vs Package

### Bundle

- 앱 번들, 프레임워크 번들, 로더블 번들 등 다양한 종류의 번들이 존재
  - 앱 번들은 앱에 대한 정보를 지니고 있는 Info.plist를 가짐
- 실행 가능한 코드와 코드에 의해 사용되는 리소스를 가진 디렉토리
- app이 대표적으로 package이자, 실행 코드와 리소스를 포함하고 있으므로 Bundle



### Package

- 다음의 조건을 만족할때 directory == 파인더가 만든 패키지
  - 디렉토리에 .app, .playground, .plugin 등의 특별한 확장자를 가진 파일이 있다.
  - 디렉터리에 document 타입으로 등록된 앱의 확장자가 존재한다.
  - 디렉터리에 그 자체를 패키지로 보여지게하는 어트리뷰트가 존재한다.
- Finder가 사용자에게 단일 파일로 보여주는 디렉토리
  - Package는 사실 디렉토리임. 근데 이걸 Finder는 단일 파일처럼 인식해서 보여줌
  - xcodeproj 파일이 그냥 파일처럼 인식되는 것이 그 예시
    - 원래 디렉토리면 해당 폴더 내부로 들어가게 해줘야하는데, 엑스코드가 실행되는 것
  - application도 패키지의 한 종류 (.app)
  - .app, .bundle, .playground, .plugin 등을 패키지로 인식



## Library vs Framework

### Framework

- Framework는 공유자원(dynamic shared library, 이미지 파일 등)을 단일 패키지 형태로 담고있는 디렉토리

- Library와 다르게 리소스를 포함할 수 있음

- Framework는 번들의 한 종류

  - Framework 내의 리소스에 접근할 수 있지만 해당 리소스가 포함된 번들(framework를 명시해주어야 함)
  - 만약 A Framework와 B Framework가 같은 리소스를 사용한다해도, 서로 다른 리소스로 인식됨

- ```swift
  let bundle = Bundle(identifier: "org.alamofire.Alamofire") // Alamofire의 Bundle Identifier
  let image = UIImage(named: "sampleImg", in: bundle, compatibleWith: nil)
  ```

  이런식으로 사용할 수 있음

- 프레임워크는 실행파일로 취급되지 않고 디렉토리로 취급됨 (앱 번들과 다르게)

- application이 특정 작업을 수행할 수 있는 기능을 제공



### static library, framework, metal library

- static library의 확장자는 .a
- dynamic library의 확장자는 .dylib
- 라이브러리랑 다르게 프레임워크는 리소스도 포함 가능(라이브러리는 소스코드만 포함 가능)
- 프레임워크는 여러 버전을 동일한 번들에 포함시킬 수 있음





### Binary

### Header

### Modulemap



### Mach-O type



## Target, Scheme, Workspace, Project, Module







## Swift Packages

가볍게 재활용 가능한 코드를 만들고 조직화하고 다른 Xcode 프로젝트로 공유하여 다른 개발자들과 함께 사용할 수 있도록 하는 툴

스위프트 패키지는 스위프트, 옵젝씨, 

SPM은 Swift 코드를 배포하기 위한 관리도구입니다.

Swift build system과 통합되어 있으며 다운로드, 컴파일, 디펜던시 링킹을 자동화해줍니다.



### Module

스위프트는 코드를 module들로 조직화합니다. 각각의 모듈은 namespace를 지정해주며 접근제어를 통해 어떤 코드가 모듈 밖에서 사용될 수 있는지를 정해줍니다.

프로그램은 모든 코드를 하나의 모듈에 가지고 있을 수도 있고, 다른 모듈을 import하여 그 모듈에 의존성을 가질 수도 있습니다. 시스템 제공 모듈을 제외하면 대부분의 의존성은 사용하기 위해선 다운로드하고 빌드되어야 합니다.

특정 문제를 해결하는 모듈은 재활용되어 다른 상황에서도 사용될 수 있습니다. 모듈은 다른 개발자의 코드 위에서 다른 기능을 직접 추가할 수 있도록 해줍니다.



### Packages

패키지는 소스파일들과 Package.swift 파일이라는 manifest 파일로 구성됩니다. manifest 파일은 패키지의 이름과 사용할 컨텐츠들을 PackageDescription 모듈을 통해 정의합니다.

패키지는 하나 이상의 target을 가지며 각각의 target은 특정한 product를 지정하며, 하나 이상의 의존성을 가질 수도 있습니다.



### Products

타겟은 라이브러리나 실행 가능한 파일을 product로 삼아 빌드합니다. 라이브러리는 다른 스위프트 코드가 import할 수 있는 모듈을 포함하며, 실행 가능한 파일은 os에 의해 실행되는 프로그램입니다.



### Dependencies

타겟의 의존성은 패키지 내의 코드가 필요로 하는 모듈입니다. 의존성은 패키지의 source의 URL과 패키지의 사용가능한 버전정보로 구성됩니다. 패키지 매니저의 역할은 프로젝트에 사용되는 의존성의 다운로드를 자동화하여 조직화하는 비용을 줄이는데 있습니다. 의존성 자기 자신도 의존성을 가질 수 있으므로 이를 종합하여 의존성 그래프가 생성됩니다. 패키지 매니저는 의존성 그래프를 만족시키기 위한 모든 자료를 다운받습니다.



### Project



### Binary

binary 파일은 바로 실행할 수 있는 파일을 뜻합니다. exe 등이 대표적인 binary file입니다.

회사가 어떤 코드를 바로 라이브러리로 제공하지 않고 binary 형태로 제공할수도 있는데 이것을 하나의 package가 사용할수도 있습니다.



## Xcode Concepts

타겟은 build할 product를 specify합니다. 또한 프로젝트나 워크스페이스 내의 파일들을 통해 



하나의 타겟이 빌드된걸 다른 타겟을 빌드할때 사용할 수 있다

이런건

Domain이라는 타겟을 먼저 빌드하고

Data라는 타겟을 빌드해야되는 상황?



### 참고한 공식문서

https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Targets.html

https://www.swift.org/package-manager/

### 참고한 블로그

https://zeddios.tistory.com/706

https://hcn1519.github.io/articles/2018-12/bundle