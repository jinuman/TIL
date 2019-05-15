# Protocol
> "A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. "

- **프로토콜은 `정의`만 하고, 이 프로토콜을 채택한 곳에서 `구현`한다.**
- 프로토콜은 해당 이름에 걸맞는 기능을 하기 위해 구현해야 하는 것들의 리스트라고 할 수 있다.
- 프로토콜은 특정 역할을 수행하기 위한 `method`, `property`, `initializer`, `subscribt` 등의 요구사항을 정의한다.
- 프로토콜은 다중 채택이 가능하다. (다중 상속과 비슷하지만 더 확장된 개념)
    - `class`, `struct`, `enum`은 프로토콜을 채택해서 프로토콜의 요구사항을 실제로 구현할 수 있다.
    - 프로토콜이 제시하는 프로퍼티, 메소드 등의 요구사항을 모두 구현해야 에러가 나지 않는다.
    - 만약 선택적인 메소드 구현을 위해서는 `@objc optional` 키워드를 이용한다.

### Property 요구 사항

- **항상 `var` 를 사용한다.**
- 오직 프로퍼티의 이름, 타입, get/set 여부만 요구된다.
- 읽기 가능은 `{ get }`, 읽기/쓰기 모두 가능은 `{ get set }`
- `{ get set }`으로 되어 있는 경우 반드시 get, set 모두 구현해야 한다.
    - **이 경우 구현하는 프로퍼티는 let, 읽기 전용 연산 프로퍼티는 될 수 없다.**
    - `{ set }`만 요구할 수는 없다.
- `{ get }`은 모든 종류의 프로퍼티에서 요구사항을 충족시킬 수 있으며, **필요하다면 `{ get set }`이 될 수도 있다.**
- For Example

```Swift
protocol Talkable {
    var topic: String { get set }
    var language: String { get }

    func talk()
    @objc optional func shout()

    init(topic: String, language: String)
}

struct Person: Talkable {
    // 프로퍼티 요구 준수
    var topic: String
    let language: String    // 상수로 읽기 전용을 구현

    // 메서드 요구 준수
    func talk() {
        print("\(topic)에 대해 \(language)로 말합니다")
    }

    // 이니셜라이저 요구 준수
    init(topic: String, language: String) {
        self.topic = topic
        self.language = language
    }
}
```

**✻ 만약 상속과 같이 사용해야 하는 경우, 상속 부분을 먼저 채택하고, 그 다음 프로토콜을 채택해야 한다.**

```Swift
class SubClass: SuperClass, SomeProtocol {
    ...
}
```

### 프로토콜의 장점

1. 여러 개의 프로토콜을 준수할 수 있기 때문에 확장성, 재 활용성이 좋다.
2. 프로토콜은 `class`타입만 가능한 상속과는 다르게 `struct`, `enum`에서도 준수할 수 있다.
3. 프로토콜의 다중 채택, extension과의 결합을 통해 강력한 효과를 얻을 수 있다.

## 다중 채택

- 객체의 기능을 세분화해서 사용하는 다중 상속과 비슷한 개념이다.
- 차이점은 상속은 대부분 참조 타입인 class 만 가능하지만, 프로토콜 다중 채택은 struct, enum 등의 타입도 가능하다.
    - class 는 참조 타입이라서 참조 추적에 비용이 많이 든다.
- 또 다른 차이점은 다중 상속은 하나의 상속 체계에서 다른 상속 체계에 속해 있는 기능을 끌어다 쓸 수 없다.
    - 프로토콜 다중 채택은 extension 을 통해 가능하다.

```Swift
protocol Bird {
    var name: String { get }
    var canFly: Bool { get }
}

protocol Flyable {
    var airspeedVelocity: Double { get }
}

struct Penguin: Bird {
    let name: String
    let canFly = false
}

struct SwiftBird: Bird, Flyable {
    var name: String {
        return "Swift \(version)"
    }

    let canFly = true
    var version: Double

    var airspeedVelocity: Double {
        return version * 1000.0
    }
}
```
- Penguin 은 전형적으로 날지 못하는 새지만, 새는 새이기 때문에 `Bird` 이지만, `Flyable` 하지는 않게 구현할 수 있다.
- 단일 채택, 단일 상속만 가능한 상황이라면, 이런 상황은 구현하기 매우 난감하다.
- 하지만 위의 코드에서 `Flyable`이라는 개념이 존재함에도 불구하고 `canFly`인지 아닌지 불필요하게 명시 중이다.
- 프로토콜 지향 프로그래밍에서 `Default Implementation` (초기 구현)을 통해 위 문제를 해결할 수 있다.

# 프로토콜 지향 프로그래밍

- extension 을 이용한 초기 구현이 핵심이다.
- 다중 채택 코드에서 `canFly` 에 Default Implementation 을 적용하면 더 깔끔하다.

```Swift
protocol Bird {
    var name: String { get }
    var canFly: Bool { get }
}

protocol Flyable {
    var airspeedVelocity: Double { get }
}

struct Penguin: Bird {
    let name: String
}

struct SwiftBird: Bird, Flyable {
    var name: String {
        return "Swift \(version)"
    }

    var version: Double

    var airspeedVelocity: Double {
        return version * 1000.0
    }
}

extension Bird {
    var canFly: Bool {
        return self is Flyable
    }
}

let penguin = Penguin(name: "penguin")
print(penguin.canFly)  // false

let swiftBird = SwiftBird(version: 5)
print(swiftBird.canFly)  // true
```

- 이런 식으로 새가 `Flyable` 을 채택한 타입이라면 canFly 를 true 로 바꿔줄 수 있다.
- 좀 더 객체 지향적인 설계가 가능하다. SOLID 참고