# Optional

## What is optional?

- 값이 있을 수도 없을 수도 있는 (nil일 수도 아닐 수도 있는) 타입이다.
- Optional 타입은 열거형이다.
```Swift
enum Optional<T> {
    case some(T)
    case none
}
```

- Optional 타입은 따로 초기화를 하지 않으면 nil로 초기화 된다.
- **초기화 없이 옵셔널 타입 선언은 `var`만 가능하다!** let은 불가능하다.
```Swift
var text1: String?
let text2: String? // error: must have an initial value
print(text1) // nil
```

- 옵셔널을 switch문으로 받아보기
```Swift
var maybeNumber: Int? = 0
switch maybeNumber {
case let .some(number):
    print(number)  // 0
case .none:
    print("nil")
}
```

## Why do we use Optional type?

- **nil 가능성을 명시적으로 표현한다.**
- 기존에 nil 로 인한 crash 문제를 compile 시점에서 막아주려는 목적으로 만들어졌다.
- `optional` 을 쓰는 이유를 담은 코드

    통상적으로 배열이 비어 있으면 -1 을 return 하도록 했었다.

    그러나 만약 list 가 minus 값들의 배열이면 -1 을 return 하는 것은 `ambiguous` 하다.

    그러므로 빈 배열이 들어오면 `return nil` 을 해주도록 하는 것이다.
    ```Swift
    func max(in list: [Int]) -> Int? {
        if list.count == 0 {
            return nil
        }
        var maxNum = Int.min
        for number in list {
            if number > maxNum {
                maxNum = number
            }
        }
        return maxNum
    }

    print(max(in: [])) // nil
    print(max(in: [-5, -3, -2, -1])) // Optional(-1)
    ```

## Optional 의 단점

> Unwrapping 을 해줘야 한다는 "귀찮음"이 있다.

- Optional 변수를 일반 변수처럼 쓰려면 `Unwrapping`을 해줘야 한다.

```Swift
var number : Int? = 3
print(number?.description)       // Optional("3")
print(number?.description ?? 0)  // 3
```

- 참고) description : Type to `String`

### ✻ nil 병합 연산자

- Optional ?? value
- Optional 값이 `nil`일 경우 value 를 return 한다.
- 띄어쓰기에 유의해야 한다.

## Unwrapping optional

- 강제 Unwrapping
- 옵셔널 바인딩
- 옵셔널 체이닝
- 참고) ***✻ Optional 값과 아닌 값을 비교할 때는 Unwrapping 안해도 된다!***

### 1. Force Unwrapping

- 옵셔널 값 뒤에 느낌표(!)를 사용하면 된다.
- 값이 존재하지 않는 옵셔널 값에 접근하려 시도하면 런타임 에러가 발생한다.
- 그러므로 항상 옵셔널 값이 nil이 아니라는 확신이 들 때만 사용해야 한다. 강제 Unwrapping을 통해 자주 보는 error
  ```
    fatal error: unexpectedly found nil while unwrapping an Optional value (lldb)
  ```

### 2. Optional Binding

- 값이 있을 때만 바인딩 되게하는 방법이다.
- if let(var), guard let(var), switch let(var)
- if 문 안에서 쉼표(,)는 && 효과를 줄 수 있다.

```Swift
var money: Int? = 10000
if let myMoney = money {
    if myMoney >= 1000 {
        print("rich")
    }
}
// 위와 같은 코드
var money: Int? = 10000
if let myMoney = money, myMoney >= 1000 {
     print("rich")
}
```

### 3. Optional Chaining

- Chaining **중간에 nil 이 하나라도 발견되면 nil 을 반환한다.**
- `Debugging` 시 어디에서 nil 이 발생했는지 알기 어려운 단점이 있다.
- Optional 내부의 내부의 내부로 Optional 이 연결되어 있을 때 유용하게 활용할 수 있다.
- 그러므로 매번 nil 확인을 하는 것이 아니라 *최종적으로 원하는 값이 있는지 없는지 확인할 때 유용하다.*

```Swift
// if let
func numberOfCharacters(of maybeString: String?) -> Int? {
    if let str = maybeString {
        return str.count
    } else { 
                return nil 
        }
}
// guard let
func numberOfCharacters2(of maybeString: String?) -> Int? {
    guard let str = maybeString else { return nil }
    return str.count
}
// optional chaining
func numberOfCharacters3(of maybeString: String?) -> Int? {
    return maybeString?.count
}
```