# Closure
> 익명함수, 즉 이름이 없는 함수 in Swift

- "FRP"에서 task(작업)의 단위로 주로 쓰인다. 
- 변수, 상수 등으로 저장할 수 있다.
- 전달인자로 전달할 수 있다.
- return 키워드 생략할 수 있다.
  - **마지막에 있는 값을 반환 값으로 인식한다.**
```Swift
{ (parameter, parameter, ...) -> (returnType) in 
    codes..
}
```

## Function vs Closure

| 비교 | Function | Closure |
| ---- | ---- | ---- |
| name | O | X |  
| `func` keyword | O | X | 
| `in` keyword | X | O |

```Swift
func giveFunc() { ... } // function
var giveNoFunc = { () -> in ... } // closure
giveFunc()   // call function
giveNoFunc() // call closure
```

## Function to Closure

```Swift
func sayHello(name: String) -> String {
    return "Hello \(name)!"
}
sayHello(name: "jinuman")
```
### 위 함수를 클로저로 바꿔보기

1. 중괄호를 제거한다.
```Swift
func sayHello(name: String) -> String
    return "Hello \(name)!"
```
2. in 키워드를 returnType 뒤에 추가한다.
```Swift
func sayHello(name: String) -> String in
    return "Hello \(name)!"
```
3. func 키워드와 함수 이름을 제거한다.
```Swift
(name: String) -> String in
    return "Hello \(name)!"
```
4. 전체를 중괄호로 감싸고 변수에 할당한다.
```Swift
var sayHelloClosure = {(name: String) -> String in
    return "Hello \(name)!"
}
sayHelloClosure("jinuman")
```
### 위에서 만들어진 클로저 축약해보기

1. return 키워드를 생략한다.
```Swift
var sayHelloClosure = {(name: String) -> String in
    "Hello \(name)!"
}
```
2. returnType 도 생략할 수 있다.
   - 이 경우 Swift에서 타입 추론을 해준다. 
```Swift
var sayHelloClosure = {(name: String) in
    "Hello \(name)!"
}
```
3. 클로저 타입을 명시해주면 parameter 생략이 가능하다.
```Swift
var sayHelloClosure: (String) -> String = {
    "Hello \($0)!"
}
```

## Closure로 프로퍼티 할당 
- **실행한 클로저의 타입은 그 클로저의 리턴 타입이다!**
- 함수형 프로그래밍에서 자주 사용되는 개념이다.

```Swift
var closure = { () -> String in
    var str = ""
    for i in 1...3 {
        str += "\(i), "
    }
    return str
}

var str = closure()
print(str)  // "1, 2, 3,"
```
위 코드와 완벽하게 같은 코드로
```Swift
var str: String = { () -> String in
    var str = ""
    for i in 1...3 {
        str += "\(i), "
    }
    return str
}()
print(str) // "1, 2, 3,"
```

* * * 
## Closure as a parameter

- 클로저는 함수의 전달인자로 자주 사용된다. - 일급 객체의 특징
- 한번 정의한 함수와 타입이 같으면 재사용할 수 있다.
```Swift
let add: (Int, Int) -> Int = { $0 + $1 }
let substract: (Int, Int) -> Int = { $0 - $1 }
let divide: (Int, Int) -> Int = { $0 / $1 }
let multiply: (Int, Int) -> Int = { $0 * $1 }
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

print(calculate(a:4, b:2, method:add)) // 6
print(calculate(a:4, b:2, method:substract)) // 2
print(calculate(a:4, b:2, method:divide)) // 2
print(calculate(a:4, b:2, method:multiply)) // 8
```
- ex) 두 정수를 제곱해서 더한 후 result에 저장
  - 위에서 정의한 calculate 함수를 재사용할 수 있다.
```Swift
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

let result = calculate(a: 3, b: 2){(left: Int, right: Int) -> Int in
    left * left + right * right
}

// return type 생략 가능
// calculate 함수가 이미 method return type을 알려주기 때문에 가능하다.
let sameAsAbove = calculate(a: 3, b: 2, method: {(left: Int, right: Int) in
    left * left + right * right
})

print(result)       // 13
print(sameAsAbove)  // 13
```

## 단축 인자 이름 $0, $1, ...

- 클로저의 매개변수 이름이 굳이 필요하지 않을 때 사용할 수 있다.
- 클로저의 __매개변수 순서대로__ `$0, $1, ...` 처럼 표현한다.
- in 키워드를 생략할 수 있다.
- 클로저는 가독성 측면에서 복잡해보일 수 있기 때문에 단축 인자이름을 통해 코드도 줄이고, 가독성도 높일 수 있다.

```Swift
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

let result = calculate(a: 3, b: 2, method: {
    return $0 * $0 + $1 * $1
})

print(result)       // 13
```

## 후행 클로저

- 클로저가 함수의 마지막 parameter라면, **parameter 이름을 생략하고 함수 소괄호 외부로 뺄 수 있다.**

```Swift
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

let result = calculate(a: 3, b: 2) {
    return $0 * $0 + $1 * $1
}

print(result)       // 13
```
