# Enum

- Swift 의 enum 은 기존 다른 언어에 비해 강력하다.
- `값 타입`이다.
- enum 자체가 하나의 데이터 타입이고, 각각의 case 는 그 자체로 고유의 값이 된다.
- enum 은 switch 문과 자주 사용될 수 있다.
- Swift 의 enum 에는 method 를 추가할 수 있다.

### How to choose a model type?
```
A, B, C 가 있을 때, 
enum : A or B or C             // ex) 사람의 성별 ( 남자 혹은 여자 )
struct, Tuple : A and B and C  // ex) 사람 한명 ( 이름, 키, 몸무게, 등등 )
```

```Swift
enum Month {
    case mar, apr, may
    case jun, jul, aug
    case sep, oct, nov
    case dec, jan, feb
    
    func printMessage() {
        switch self {
        case .mar, .apr, .may:
            print("Spring")
        case .jun, .jul, .aug:
            print("Summer")
        case .sep, .oct, .nov:
            print("Autumn")
        case .dec, .jan, .feb:
            print("Winter")
        }
    }
}
print(Month.dec)    // dec
Month.mar.printMessage()    // Spring
```

### 연관값 바인딩

- 연관값이란? 밑에서 (Double), (version: Int) 와 같은 것을 말한다. 

```Swift
enum OS {
    case macOS(Double)
    case linux(String, Int)
    case windows(version: Int)
}

let highSierra: OS = .macOS(10.13)
let fedora12: OS = .linux("Fedora", 12)
let windows10: OS = .windows(version: 10)

switch fedora12 {
case .macOS(let version):
    print("Mac OS \(version)")
// 위랑 같은 표현
case let .linux(distribution, release):
    print("Linux \(distribution) - \(release)")
case let .windows(version):
    print("Windows \(version)")
}  // "Linux Fedora - 12"
```

### rawValue

- Hashable 프로토콜을 따르는 모든 타입이 `rawValue`의 타입으로 지정될 수 있다.
- __연관값이 없는 enum만 가능하다.__
- __case 별로 각각 다른 값을 가져야 한다.__
```Swift
enum Fruit: Int {
    case apple = 0
    case grape = 1
    case peach      // 자동으로 1씩 증가한다.
}

var num: Int = 0
if let fruit: Fruit = Fruit(rawValue: num) {
    print("rawValue \(num) : \(fruit)")
} else {
    print("rawValue \(num) : 해당하는 케이스가 없습니다.")
}  // "rawValue 0 : apple"
print(Fruit.peach.rawValue)     // 2
```

- 원시값을 지정해주지 않으면 case 값 그대로 가져온다.

```Swift
enum School: String {
    case elementary = "초등"
    case middle = "중등"
    case high = "고등"
    case univ
}
print(School.univ.rawValue) // univ
```

### Optional is an enum type with 2 cases!

- 밑에 처럼 활용할 수 있다.

```Swift
let age: Int? = 20

switch age {
case .none: // `nil`인 경우
    print("나이 정보가 없습니다.")
    
case .some(let x) where x < 20:
    print("청소년")
    
case let .some(x) where x < 65:
    print("성인")
    
default:
    print("어르신")
}  // "성인"
```
