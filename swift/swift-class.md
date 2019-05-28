# Class

> Apple framework 의 대부분의 뼈대는 class 로 구성되어 있다.

- **Class 는 참조 타입 / Struct, enum 은 값 타입이다.**
- **class 는 let 으로 인스턴스를 선언 해도 가변 프로퍼티의 값을 변경할 수 있다.**
    - class는 참조타입이기 때문이다.
- class 안에서는 프로퍼티들을 초기화 해주어야 한다. struct 는 init 이 없어도 사용할 수 있다.
    - 만약 Codable을 준수한다면 프로퍼티들을 초기화 해주지 않아도 된다.

```Swift
class Sample {
    var mutableProperty: Int = 100 // 가변 프로퍼티
    let immutableProperty: Int = 100 // 불변 프로퍼티
    static var typeProperty: Int = 100 // 타입 프로퍼티

    // 인스턴스 메서드
    func instanceMethod() {
        print("instance method")
    }

    // final 키워드 : 재정의 방지
    final func sayHello() {
        print("Hello")
    }

    // 타입 메서드
    // 상속 시 재정의 불가 메서드 - static
    static func typeMethod() {
        print("type method")
    }
    // 상속 시 재정의 가능 메서드 - class
    class func classMethod() {
        print("class method")
    }
}

// let으로 인스턴스를 만들어도 가변 프로퍼티 변경 가능 - struct와의 차이점
let immutable: Sample = Sample()
immutable.mutableProperty = 200
print(immutable.mutableProperty)    // 200
```

## Initializer

- 설계 시 프로퍼티의 초기 값이 꼭 필요하지 않을 때 Optional 변수를 사용하면 초기화를 나중에 해줘도 된다.

```Swift
class User {
    let name: String
    let email: String
    var nickname: String? // 옵셔널 선언 - init 필요 없음

    init(name: String, email: String) {
        self.name = name
        self.email = email
    }

    init(name: String) {
        self.name = name
        self.email = "I don't have email."
    }
}
```

### Failable Initializer

> 실패 가능한 이니셜라이저

- **인스턴스 생성 시 매개변수의 초기 값에 조건을 줄 경우 많이 사용한다.**
- 이 경우 인스턴스의 클래스 타입은 Optional 이 되므로 사용 시 unwrapping 이 필요하다.

```Swift
class Person {
    let name: String
    let age: Int
    var nickname: String?

    init?(name: String, age: Int) {
        if (0...120).contains(age) == false {
            return nil
        }
        if name.count == 0 {
            return nil
        }
        self.name = name
        self.age = age
    }
}

let person: Person? = Person(name: "jinu", age: 290) // nil
```

### convenience init

> 보조 이니셜라이저

- 클래스 안에서 Designated init 이 먼저 생성된 경우에 사용 가능하다.
- `convenience init`은 **같은 클래스에서 한 init 이 다른 init 을 호출 해야한다.**

```Swift
protocol EntryType: class {
    var id: UUID { get }
    var createdAt: Date { get }
    var text: String { get set }
}

class Entry: EntryType {
    let id: UUID
    let createdAt: Date
    var text: String    // 위에서 set으로 설정했기 때문에 var 선언

    init(id: UUID = UUID(), createdAt: Date = Date(), text: String) {
        self.id = id
        self.createdAt = createdAt
        self.text = text
    }
}

extension Entry {
    // dictionary의 어떤 값이 들어있냐에 따라 만들 수도 있고, 못 만들수도 있다.
    // convenience + failable initializer
    convenience init?(dictionary: [String: Any]) {
        guard
            let uuidString = dictionary["uuidString"] as? String,
            let uuid = UUID(uuidString: uuidString),
            let createdAtTimeInterval = dictionary["createdAt"] as? Double,
            let text = dictionary["text"] as? String
            else { return nil }

        self.init(id: uuid, createdAt: Date(timeIntervalSince1970: createdAtTimeInterval), text: text)
    }
}
```

### deinit

- 클래스의 인스턴스가 메모리에서 해제되는 시점에 호출된다.
- Retain Cycle 을 확인할 때 사용하면 좋다.
- **해제되는 시점에 해야할 일을 구현할 수 있다.**

```Swift
class Person {
    deinit {
        print("\(self.name)님이 죽었습니다.")
    }

    let name: String = "jinu"
    let age: Int = 29  
}

var person: Person? = Person()
person = nil    // "jinu님이 죽었습니다."
```

## Inheritance

- Swift 에서는 **단일 상속**만 가능하다.
- 부모 class 에서 기능을 추가하거나 override 해서 새로운 클래스를 정의하는 것이다.
- 부모 class 의 기능을 재사용 할 수 있다는 장점이 있다.
- **설계 시 중복되는 것들을 부모 클래스에 정의하면, 자식 클래스에서는 다시 작성하는 일을 피할 수 있다.**
- Application 이 커지면 커질수록 **유지 보수가 어렵다는 단점이 있다.**
- 클래스 상속과 프로토콜 채택을 동시에 한다면, *상속 받는 부모 클래스를 먼저 써준 다음 프로토콜을 쓰는 것으로 구분한다.*

- For example

```Swift
class Vehicle {
    var currentSpeed = 0.0      // stored property
    var description: String {    // computed property
        return "\(currentSpeed) miles per hour"
    }

    func makeNoise() {
        print("noiseless")
    }
}

// Vehicle 을 상속 받은 Train
class Train: Vehicle {
    override init() {
        print("This is train")
    }

    override func makeNoise() {
        print("Choo Choo")
    }
}

// Vehicle 을 상속 받은 Car
class Car: Vehicle {
    var gear = 1
    override init() {
        print("This is car")
    }
    init(gear: Int) {
        self.gear = gear
    }
}

// Car 를 상속 받은 SportsCar
class SportsCar: Car {
    var currentNumberOfPassengers = 0
    override var description: String {
        return """
        SportsCar has \(gear) gear,
        \(currentSpeed) miles per hour, \(currentNumberOfPassengers) passengers
        """
    }
    // init 을 구현 안해도, 부모의 init 을 불러올 수 있다.
}

let train = Train() // "This is train"
train.makeNoise()   // "Choo Choo"

let car = Car()    // "This is car"
car.currentSpeed = 100
print(car.description) // "100.0 miles per hour"

let sportsCar = SportsCar(gear: 4)
sportsCar.currentNumberOfPassengers = 2
sportsCar.currentSpeed = 60
print(sportsCar.description) // SportsCar has 4 gear,\n60.0 miles per hour, 2 passengers
```