# Swift

> safe, fast, expressive

## Swift의 특징

- 안정성 (safe)

> 프로그래밍을 하는 중에 실수를 엄격한 문법으로 미연에 방지한다.optional, guard 구문, 오류 처리, 강력한 타입 통제 등을 통해 안전한 프로그래밍을 구현한다.

- 신속성 (fast)

> C언어 기반인 C++, Objective-C를 대체하는 목적으로 만들어졌다. 실행 속도의 최적화 뿐만 아니라 컴파일러의 지속적인 개량을 통해 더 빠른 컴파일 성능을 낸다.

- 표현성 (expressive)

> 보기 좋은 문법을 구사한다. 현대적이고 세련된 문법을 구사한다. 계속된 업데이트를 통해 발전 중이다.

- Swift : 정적 타입 언어, Objective-C : 동적 타입 언어

> 정적 타입 언어란? 타입을 컴파일 시 결정하는 언어이다.

- ARC( Automatic Reference Counting ) 이용한 메모리 관리 자동화

- 언어에서 멀티 패러다임을 지원한다.
    - OOP, FP( 함수형 프로그래밍 )

## Constants and Variables

The value of a constant can’t be changed once it’s set, whereas a variable can be set to a different value in the future.
```Swift
let constant: String = "차후에 변경이 불가능한 상수 let"
var variable: String = "차후에 변경이 가능한 변수 var"
```

## Data Type

- Swift의 대부분의 데이터 타입들은 `struct`로 되어있다.

```Swift
public struct Int

public struct Double

public struct String

public struct Dictionary<Key : Hashable, Value>

public struct Array<Element>

public struct Set<Element : Hashable>
```

### String

- Multiline String Literals

```Swift
let str = "Hello, playground.\nGoodbye Objective-C\n\"Hi Swift\""
let str2 = """
Hello, playground.
Goodbye Objective-C
"Hi Swift"
"""
str == str2
```

- 빈 변수 선언

```Swift
let emptyString = ""
let anotherEmptyString = String()
```

### Any

- Swift의 모든 타입을 지칭하는 키워드

### AnyObject

- 모든 클래스 타입을 지칭하는 **프로토콜**

### nil

- "**없음"** 을 의미하는 키워드

## Basic Operator

- `a++`, `++a` 이런 증감연산자는 지원 안한다. 그러므로 `a += 1` 해줘야 한다.
- **closed range operator** : 1…5
    - 1 부터 5까지
- **half-open range operator** : 1..<5
    - 1 부터 4까지

## Conditional Statements

1. if-else
2. **switch** : 정수 외에 대부분의 기본 타입을 사용 가능
    - Swift 의 switch 는 강력하다.
    - switch 문에서 `break`는 꼭 필요하지 않다.
    - 대신 `default`문은 필요하다. 마지막에 작성해야 한다.
    - `break` 는 switch 문을 나가는 것이다. switch 문을 감싼 for 문을 나가는 것이 아니다.
- if and switch

```Swift
// if statement
if age < 3 {
    print("Baby")
} else if 3 <= age && age < 20{
    print("Child")
} else {
    print("Adult")
}

// switch statement
switch age {
case 0, 1, 2:
    print("Baby")
case 3...19:
    print("Child")
default:
    print("Adult")
}
```

## Control flow

1. `for-in` Loop
2. `forEach` loop
3. `while` loop
4. `repeat-while` loop
    - 다른 언어의 do-while 이랑 하는 짓은 같음
- 그냥 단순히 반복만 해주고 싶을 때

```Swift
for _ in 1...5 {
    print("up!")  // up! 5 times
}
```

- **`stride` 함수를 이용하면 index 접근이 더 편하다.**
    - through : 끝 포함, to : 끝 포함 (x)
    - Fatal error: Stride size must not be zero.
    - 즉, by: 0 (x)

```Swift
let minutes = 60
let minuteInterval = 5
for tickMark in stride(from: 0, through: minutes, by: minuteInterval){
    print(tickMark)     // 0, 5, 10, ..., 55, 60
}
for tickMark in stride(from: 0, to: minutes, by: minuteInterval){
    print(tickMark)     // 0, 5, 10, ..., 55
}
```
## Function

```Swift
func 함수이름(전달인자레이블 이름: 타입, _ 이름: 타입) -> 반환타입 {
  함수 구현
  return 반환값
}
```

### Argument Label ( 전달인자 레이블 )

- **함수 내부에선 `Parameter name`을, 함수를 호출할 때는 `Argument Label`을 사용한다.**
- 코드의 가독성을 증가시킨다. 좀 더 사람의 언어적인 코딩이 가능하다.
- If you don’t want an argument label for a `parameter`, write an underscore _ instead of an explicit argument label for that parameter.

### Variadic Parameters ( 가변 매개변수 )

- A variadic parameter accepts zero or more values of a specified type.
- **함수당 하나만 가질 수 있다.**

```Swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    if numbers.count == 0 {
        return total
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)    // 3.0
arithmeticMean(3, 8.25, 18.75)   // 10.0
arithmeticMean()    // 0
```

## Function as a type

- Swift는 함수형 프로그래밍 패러다임을 포함하는 다중 패러다임 언어이다.
- Swift의 함수는 **일급 객체**이다. 변수에 저장, 매개변수 전달이 가능하다.
- **반환 타입을 생략할 수 없다.**

```Swift
func greeting(to friend: String, from me: String) {
    print("Hello \(friend)! I'm \(me)")
}

var someFunction: (String, String) -> Void = greeting(to: from:)
someFunction("pogba", "jinu")   // "Hello pogba! I'm jinu"

func runAnother(function: (String, String) -> Void) {
    function("robert", "jinu")
}
runAnother(function: someFunction)  // "Hello robert! I'm jinu"
```

---

### State ( 상태 ) : 프로그램 안에서 변할 수 있는 요소

- 정말 변해야 하는 것만 변할 수 있게 만들수록 프로그램은 단순해진다.
- 최대한 상태를 많이 만들지 말자.
- 최대한 var 보다 let 을 쓰는 것이 앱의 성능이 좋아진다.

### 주석 사용을 피해야 하는 이유

- 구현된 코드와 내용이 다른 주석은 오히려 독이다.
- 코드의 변경에 따라 주석도 변경해야 하는 불편함이 있다.
- **주석 없이도 의도와 동작을 알 수 있도록 코드를 작성하는 것이 좋다. ( Show, don’t tell )**
- 그럼에도 주석을 쓰는 경우 : 코드 만으로는 다른 개발자와 협업에 있어서 이해하기 힘든 로직이 있을 때

### 타입 추론

- 컴파일러가 `lhs` (left-hand-side), `rhs` (right-hand-side)의 타입 방정식을 풀어서 적절한 타입을 계산
- 그러나 타입을 써주는 것이 성능 상에도 좋고 협업 상에도 좋다.

### Type Aliases : 타입 별칭
```Swift
typealias StatusCode = Int
// 위 처럼 선언하면 Int 와 StatusCode 를 서로 교환해가며 쓸 수 있다. 말그대로 별칭
```
### 타입 출력하기
```Swift
print("variable : \(type(of: variable))")
```