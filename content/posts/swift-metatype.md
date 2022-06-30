---
title: "Swift Metatype"
date: 2022-06-30T13:39:58+09:00
draft: false
---

Swift의 Metatype은 Class, Struct, Enumeration등의 Type에 관한 정보를 가진다. Java의 refelection과 유사한 개념이다. Metatype을 이용하여 class members(propeties, methods)를 접근할수 있다. Facotry Method, Json Decoder 등에서 사용한다. 

예제에서 사용할 Class를 정의한다. static 및 instace members를 포함하는 간단한 class이다. 
```swift
class MetaUser {
    static let kind : String = "Human"
    var name : String
    var age : Int
    
    init(name : String = "default", age : Int = 0) {
        self.name = name
        self.age = age
    }
    
    static func  newInstance() -> MetaUser {
        return MetaUser()
    }
    
    func info() -> String {
        return "\(self.name) \(self.age)"
    }
}
```

## Dynamic Metatype
- 인스턴스의 Metatype은 typeof 함수를 사용하여 runtime에 얻을 수 있다. 
- Class의 Metatype의 Type: SomeClass.Type
- protocol Metatype의 Type : SomeProtocol.Protocol
- Metatype으로 static members를 접근 가능하다.
- instance member 접근시 컴파일 오류가 발생한다. 

```swift
class StudyMetatype: XCTestCase {
  func testDynamicDataType() {
        // Given
        let user = MetaUser(name: "Bob", age: 23)
        
        // When
        let type = type(of: user)
        //type.name // compile error : can not access instance member
        //type.info()
        
        // Then
        XCTAssertTrue(type is MetaUser.Type)
        XCTAssertEqual(type.kind, MetaUser.kind)
        XCTAssertTrue(type.newInstance() is MetaUser)
    }
}
```

## Dynamic Metatype

-  Someclass.self를 활용하여 인스턴스없이 Compile Time type 정보를 알아낼 수 있다. MetaType의 인스턴스 value 이다.

```swift
class StudyMetatype: XCTestCase {
    func testDynamicMetaType() {
        // given
        let type  = MetaUser.self
    
        // When
        // Then
        XCTAssertTrue(type is MetaUser.Type)
        XCTAssertEqual(type.kind, MetaUser.kind)
        XCTAssertTrue(type.newInstance() is MetaUser)
    }
}    
```


## References

* https://docs.swift.org/swift-book/ReferenceManual/Types.html
* https://swiftrocks.com/whats-type-and-self-swift-metatypes
* https://medium.com/swiftcraft/introduction-to-swift-metatypes-21949842d7a
