# Class

- **Class는 참조 타입, Struct, enum은 값 타입이다.**
- Apple 프레임워크의 대부분의 뼈대는 모두 클래스로 구성
- 따라서 class는 let으로 인스턴스를 선언해도 가변 프로퍼티의 값을 변경할 수 있다.
- class 안에서는 프로퍼티들을 초기화 해주어야 한다. struct는 안해줘도 된다.
  - 만약 Codable을 준수한다면 프로퍼티들을 초기화 안해줘도 된다.

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

- 설계 시 프로퍼티의 초기값이 꼭 필요하지 않을 때 옵셔널 사용
```Swift
class Person {
    let name: String
    let age: Int
    var nickname: String?
    // var - init 필요 없음
    // let - init 필요
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

#### 실패 가능한 Initializer
- **인스턴스 생성 시 매개변수 초기값에 조건을 줄 경우 많이 사용**
- 이 경우 인스턴스의 클래스 타입은 옵셔널이 된다.
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

#### convenience init
- 클래스 안에서 Designated init이 먼저 생성된 경우 사용 가능
- 같은 클래스에서 매개변수가 다른 init을 호출할 때 붙이는 키워드

```Swift
class Person {
    let name: String
    let age: Int
    var nickname: String?
    
    convenience init(name: String, age: Int, nickname: String) {
        self.init(name: name, age: age)  // 밑에서 만든 Designated init 호출
        self.nickname = nickname
    }
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```
#### deinit
- 클래스의 인스턴스가 메모리에서 해제되는 시점에 호출된다. 
- 해제되는 시점에 해야할 일을 구현할 수 있다.
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

- Swift에서는 단일 상속만 가능하다.
- 기존의 클래스에서 기능을 추가하거나 재정의해서 새로운 클래스를 정의하는 것이다.
- 기존의 클래스를 재활용할 수 있다는 장점이 있다.
- **설계 시 중복되는 것들을 부모클래스에 정의해놓으면, 자식 클래스에서는 다시 작성하는 일을 피할 수 있다.**
- 앱이 커지면 커질수록 유지보수가 어렵다는 단점이 있다.
- _클래스 상속과 프로토콜 채택을 동시에 한다면, 상속 받는 부모 클래스를 먼저 써준 다음 프로토콜을 쓰는걸로 구분한다._

##### Example
```Swift
class Vehicle {
    var currentSpeed = 0.0      //stored property
    var description:String {    //computed property
        return "\(currentSpeed) miles per hour"
    }
    func makeNoise() {
        print("noiseless")
    }
}

// Vehicle을 상속 받은 Train
class Train : Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}

// Vehicle을 상속 받은 Car
class Car : Vehicle {
    var gear = 1
    override init() {
        print("Car")
    }
    init(newGear: Int) {
        gear = newGear
    }
}

// Car를 상속 받은 SportsCar
class SportsCar: Car {
    var currentNumberOfPassengers = 0
    override var description: String {
        return """
        miles per hour : \(currentSpeed), number of passengers : \(currentNumberOfPassengers)
        """
    }
}

let someTrain = Train()
someTrain.makeNoise()   // "Choo Choo"

let car1 = Car()
print(car1.gear)     // 1
car1.currentSpeed = 100
print(car1.currentSpeed) // 100
let car2 = Car(newGear: 4)
print(car2.gear)    // 4
let car3 = SportsCar()
car3.currentNumberOfPassengers = 2
car3.currentSpeed = 60
print(car3.description) // "miles per hour : 60.0, number of passengers : 2"
```

