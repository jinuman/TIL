# Extension
> class, struct, enum, protocol 타입에 새로운 기능을 추가  
> 구현된 소스를 모르는 타입에도 기능 확장이 가능

- 타입만 알고 있다면 그 타입에 자신이 만든 기능을 추가해서 사용할 수 있다.
- ex) Int 타입에 자신이 만든 프로퍼티 추가할 수 있다. 

### Property, Method 추가

- 프로퍼티 관련해서는 **computed property**만 추가해서 사용하는 것이 가능하다.
- <u>`stored property`, `property observer`는 extension이 허용되지 않는다. </u>

```Swift
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    func repeatFunc(task: () -> ()) {
        for _ in 0..<self {
            task()
        }
    }
}

extension String {
    func mixUppercasedAndLowercased() -> String {
        var result: String = ""
        for (index, element) in self.enumerated() {
            if index.isEven {
                result += String(element).uppercased()
            } else {
                result += String(element).lowercased()
            }
        }
        return result
    }
}

print(2.isEven)     // true
3.repeatFunc {
    print("Hello world!")  // "Hello world!" 3번 출력
}
print("Hey!Hey!Hey!Hey!".mixUppercasedAndLowercased())  // "HeY!HeY!HeY!HeY!"
```

### Init 추가

- 타입에 새로운 init을 추가할 수 있다. 
- **deinit은 추가할 수 없다!** deinit은 기존 class scope 안에서만 가능하다.

```Swift
extension String {
    init(intTypeNumber: Int) {
        self = "\(intTypeNumber)"
    }
    init(doubleTypeNumber: Double) {
        self = "\(doubleTypeNumber)"
    }
}

let stringFromInt: String = String(intTypeNumber: 100)
print(stringFromInt)    // "100"
let stringFromDouble: String = String(doubleTypeNumber: 3.14)
print(stringFromDouble) // "3.14"
```


### Nested Types

- extension으로 class, struct, enum을 추가해 nested type을 구성할 수 있다. 

```Swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x < 0:
            return .negative
        default:
            return .positive
        }
    }
}

func getIntegerKinds(numbers: [Int]) -> [String] {
    var result = [String]()
    for number in numbers {
        switch number.kind {
        case .negative:
            result.append("-")
        case .zero:
            result.append("0")
        case .positive:
            result.append("+")
        }
    }
    return result
}

print(getIntegerKinds(numbers: [1, -1, 0, 2, 3, -30, 100])) // ["+", "-", "0", "+", "+", "-", "+"]
```
