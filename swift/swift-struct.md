# Struct

- Swift 의 대부분의 데이터 타입들은 `struct`로 되어있다. Int, String, Double 등등
- 값 타입이다.

```Swift
struct Sample {
    var mutableProperty: Int = 100 // 가변 프로퍼티
    let immutableProperty: Int = 100 // 불변 프로퍼티
    static var typeProperty: Int = 100 // 타입 프로퍼티

    // 인스턴스 메서드
    func instanceMethod() {
        print("instance method")
    }
    // 타입 메서드
    static func typeMethod() {
        print("type method")
    }
}
```

- **struct 는 인스턴스를 만들 때 let으로 선언하면 가변 프로퍼티라도 변경할 수 없다.**
    - class는 참조 타입이기 때문에 변경 가능하다.

```Swift
let immutable: Sample = Sample()
immutable.mutableProperty = 200 // error!
```

### When to use struct?

- 연관된 값들을 모아서 하나의 데이터 타입으로 묶어서 관리하고 싶을 때
- 다른 객체, 함수 등으로 전달될 때 **참조가 아닌 복사를 원할 때**
- 상속이 필요 없을 때 (extension 으로 확장 가능)

### Type property and method

- 위에 Sample 이라는 타입 자체가 사용할 수 있는 프로퍼티나 메서드를 말한다.
- 인스턴스를 만들지 않아도 된다.
- 인스턴스에서 타입 프로퍼티나 타입 메서드를 사용하는 것은 불가능하다.

```Swift
Sample.typeProperty = 200
Sample.typeMethod() // "type method"
immutable.typeMethod()  // error!
```

- 키워드는 ` `를 사용하면 변수로 사용할 수 있다.
- Struct 는 Class 와 달리 **initializer 가 꼭 필요하지 않다.**

```Swift
struct Student {
    let name: String
    let `class`: String
}

let student = Student(name: "jinu", class: "A반")
print(student.name)     // "jinu"
print(student.class)    // "A반"
```