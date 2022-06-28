---
title: "Swift"
date: 2022-06-24T17:37:56+09:00
draft: false
---

## Unit Testing
Swift 문법 공부를 위해서 Unit Test 프레임워크의 기본적인 사용법을 우선 정리. 유닛 테스트로 기본 문법 내용을 체계적으로 정리 가능

```swift
import XCTest // unit test 프레임워크
@testable import Basic // app의 함수 호출을 위해서 testable import 실행

class SudyUnitTestEssentials: XCTestCase {
    func testEqual() throws {
        // given
        let number = 1
        let expect = 1
        //when
        //then
        XCTAssertEqual(number, expect) // 값이 동일한지 확인
    }
    func testNotEual() throws {
        // given
        let number = 1
        let expect = 2
        // when
        // then
        XCTAssertNotEqual(number, expect)
    }
}
```

- 기본 테스트 결과 확인 함수
  - XCTAssertEqual
    - 1st,2nd Parameters가 == 인지 확인   
  - XCTAssertNotEqual
    - 1st,2nd Parameters가 != 인지 확인   

- 실행 키보드 shortcut
  - Selected 테스트 케이스 실행
  - Ctrl + Option + Cmaand + U

## Functions
- 함수 파라미터 형식
  - label parameterName : Type = "default parameter value"
  - label은 함수 호출시 사용하는 일종의 alias 개념으로 함수 호출을 영어 문장처럼 표현 가능
  - 함수를 파라미터로 전달 가능

```swift
func say(_ something : String = "sth", to somebody: String = "sb", callback: (String) -> Void) -> String {
    let words = "Say \(something) to \(somebody)"
    callback(words)
    return words
}
```

- 1st parameter
  - 함수 호출시 label을 생략하기 위해서 _(underscore) 사용
- 2nd Parameter
  - label 사용. label은 함수 외부(호출하는 위치) 파라피터 이름은 함수 내부에서 사용
- 3rd Parameter
  - 함수를 인자로 받아서 실행
  - 1개의 String을 인자로 받고 리턴은 없는 함수

- 함수 호출 테스트 코드
```swift
func stringPrinter(result : String) {
    print(result)
}

class StudyFunctions: XCTestCase {
    func testFunctions() {
        // given
        let something = "hi"
        let somebody  = "him"
        let expect = "Say \(something) to \(somebody)"
        
        // when
        let result = say(something, to: somebody, callback: stringPrinter)
        
        // Then
        XCTAssertEqual(result, expect)
    }
    
    func testFunctionalParameter() {
        // given
        let something = "hi"
        let somebody  = "him"
        let expect = "Say \(something) to \(somebody)"
        
        let sayFunc : (String, String, (String) -> Void) -> String
        sayFunc = say
        
        // when
        let result = sayFunc(something, somebody, stringPrinter)
        
        // then
        XCTAssertEqual(result, expect)
    }
}
```

### refs
- https://docs.swift.org/swift-book/LanguageGuide/Functions.html