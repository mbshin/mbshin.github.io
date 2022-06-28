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
  - Ctrl + Option + Command + U

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


## Struct

- struct는 class와 동일하게 properties와 methods를 포함
- value type으로서 모든 값이 복제된 후 다른 변수나 파라미터에 할당
- 기본적으로 strut를 사용하는 것을 권장. 상속, 형변환 등 클래스 기능이 꼭 필요한 경우에만 class 사용

```swift
struct User {
    var username : String
    var email : String
    var age : Int
}

class StudyStruct: XCTestCase {
    func testStructBasic() {
        // given
        let username = "first"
        let email = "first@mail.com"
        let age = 10
        
        // when
        let userA = User(username: username, email: email, age: age)
        
        // then
        XCTAssertEqual(userA.age, age)
    }
}
```

- Equivalence(==) 연산을 위해서는 Equatable Protocol 구현 필요
- Protocol은 Java의 인터페이스와 유사
- Struct는 Identy Operator(===) 사용 불가.
- Value Type으로 인하여 변수에 할당시 모든 properties가 복제된 인스턴스 생성됨

```swift
struct EquatableUser : Equatable {
    var username : String
    var email : String
    var age : Int
    
     // == 연산 지원
    static func == (lhs: EquatableUser, rhs: EquatableUser) -> Bool {
        return lhs.age == rhs.age && lhs.email == rhs.email && lhs.username == rhs.username
       }
}
```

```swift
    func testStructEquality() {
        // Given
        let username = "first"
        let email = "first@mail.com"
        let age = 10
        
        // when
        let userA = EquatableUser(username: username, email: email, age: age)
        let userB = EquatableUser(username: username, email: email, age: age)
    
        XCTAssertTrue(userA == userB) // Equatable Protocol 구현시 Equivalence Operators(==) 연산 가능
    }
    func testStructValueType() {
        // Given
        let username = "first"
        let email = "first@mail.com"
        let age = 10
        
        // when
        let userA = EquatableUser(username: username, email: email, age: age)
        var userB = userA // Value Type - 복제 진행
        userB.age = 20
    
        XCTAssertNotEqual(userA, userB)
    }
```