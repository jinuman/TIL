# Enum

- Swift 의 enum 은 기존 다른 언어에 비해 강력하다.
- `값 타입`이다.
- enum 자체가 하나의 데이터 타입이고, 각각의 case 는 그 자체로 데이터 타입이고, 고유의 값이다.
- case 자체가 고유의 값이라서 switch 문과 많이 활용된다.
- enum 안에 method 를 추가할 수 있다.
- enum 안에 **저장 프로퍼티는 만들 수 없지만, 연산 프로퍼티는 만들 수 있다.**

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
    
    var message: String {
        switch self {
        case .mar, .apr, .may:
            return "Spring"
        case .jun, .jul, .aug:
            return "Summer"
        case .sep, .oct, .nov:
            return "Autumn"
        case .dec, .jan, .feb:
            return "Winter"
        }
    }
}

Month.mar.printMessage()    // Spring
print(Month.jun.message)    // Summer
```

## How to choose a Model type?

- A, B, C 가 있을 때
- enum : A or B or C
    - ex) 사람의 성별 ( 남자 or 여자 )
- struct, tuple : A and B and C
    - ex) 사람 한명 ( 이름, 키, 몸무게, 등등)

## enum 을 사용하면 좋은 경우

- 다른 타입으로 지정해야 되는 것을 **단일 타입 내에 다른 사례로 처리하고 싶을 때**

```Swift
enum Barcode {
  case upc(Int, Int, Int, Int)
  case qrCode(String)
}
// 바코드와 QR코드를 각각 구조체나 클래스로 표현한다면 다른 타입이 되지만
// enum 을 사용하면 코드 내에서 이 둘을 같은 타입으로 다루는 것이 가능해진다.

var productBarcode: Barcode = .upc(3, 25, 153, 9)
productBarcode = .qrCode("helloworld")

switch productBarcode {
case .upc(let a, let b, let c, let d):
    print("UPC: \(a), \(b), \(c), \(d)")
case let .qrCode(message):
    print("QRCode: \(message)")
}
```

- enum 타입이 여러 case 중에 하나의 case 를 표현해야 하고
- case 자체도 표현 해주고 싶을 때
- 프로토콜의 `property` 로 `{ get set }` 해주고 싶을 때

- 프로젝트에서 enum 활용 사례

```Swift
protocol Settings {
    var dateFormatOption: DateFormatOption { get set }
    var fontSizeOption: FontSizeOption { get set }
}

// env 에 주입시킬 세팅, 이걸 UserDefaults 로 바꿀 수 있다.
// extension UserDefaults: Settings
class InMemorySettings: Settings {
    var dateFormatOption: DateFormatOption = .default
    var fontSizeOption: FontSizeOption = .default
}

protocol SettingsOption {
    static var title: String { get }
    static var `default`: Self { get }
    static var all: [Self] { get }
}

enum FontSizeOption: CGFloat, SettingsOption {
    case small = 12
    case medium = 16
    case large = 20

    static var title: String {
        return "글자 크기"
    }
    static var `default`: FontSizeOption {
        return .medium
    }

    static var all: [FontSizeOption] {
        return [.small, .medium, .large]
    }

    var description: String {
        switch self {
        case .small: return "작게"
        case .medium: return "중간"
        case .large: return "크게"
        }
    }
}
```

## 연관 값 Binding

- 위에 바코드 예제처럼 같은 타입의 값을 특정 case 에서 용도에 맞게 사용하는 것이 가능하다.
- 연관 값이란? 밑에서 (Double), (version: Int) 와 같은 것

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

- `Hashable` 프로토콜을 따르는 모든 타입이 `rawValue`의 타입으로 지정될 수 있다.
- **연관값이 없는 enum만 가능하다.**
- **case 별로 각각 다른 값을 가져야 한다.**

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

- rawValue 를 지정해주지 않으면 case 값 그대로 가져온다.

```Swift
enum School: String {
    case elementary = "초등"
    case middle = "중등"
    case high = "고등"
    case univ
}
print(School.univ.rawValue) // univ
```

### Optional is an enum type with 2 cases

- Optional 타입을 밑에 처럼 활용할 수 있다.

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