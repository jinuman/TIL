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
- **초기화 없이 옵셔널 타입 선언은 var만 가능하다!** let은 불가능하다.
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

## Why we use optional type?

- **nil 가능성을 명시적으로 표현한다.**    
  nil 가능성을 문서/주석으로 작성하지 않고 코드로 표현 가능
- **기존에 nil로 인한 crash 문제를 compile 시점에서 막아주려는 목적이다!**
  - Safe


  
```Swift
var number : Int? = 3 
print(number?.description)  // Optional("3")
```
- 참고) description : Type to `String`

- Optional 변수를 일반 변수처럼 쓰려면 Unwrapping을 해줘야 한다.

## Unwrapping optional

- 강제 Unwrapping
- 옵셔널 바인딩
- 옵셔널 체이닝  

- 참고) _**✻ Optional 값과 아닌 값을 비교할 때는 Unwrapping 안해도 된다!**_

#### 1. 강제 Unwrapping
- 옵셔널 값 뒤에 느낌표(!)를 사용하면 된다.
- 값이 존재하지 않는 옵셔널 값에 접근하려 시도하면 런타임 에러가 발생한다.
- 그러므로 항상 옵셔널 값이 nil이 아니라는 확신이 들 때만 사용해야 한다.
강제 Unwrapping을 통해 자주 보는 error
```
fatal error: unexpectedly found nil while unwrapping an Optional value (11db)
```

#### 2. 옵셔널 바인딩
- 값이 있을 때만 바인딩 되게하는 방법이다. 
- if let(var), guard let(var), switch let(var)

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
// 쉼표(,)는 && 효과를 줄 수 있다.
```
#### 3. 옵셔널 체이닝
- **체이닝 중간에 nil이 하나라도 발견되면 nil을 반환한다.**
- 옵셔널의 내부의 내부의 .. 내부로 옵셔널이 연결되어 있을 때 유용하게 활용할 수 있다.
- 그러므로 매번 nil 확인을 하는 것이 아니라 최종적으로 원하는 값이 있는지 없는지 확인할 때 유용하다.
```Swift
// if let
func numberOfCharacters(of maybeString: String?) -> Int? {
    if let str = maybeString {
        return str.count
    } else {
return nil }
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

#### ✻ nil 병합 연산자
- Optional ?? value
- Optional 값이 nil일 경우 value를 return 한다.
- 띄어쓰기에 유의해야 한다.

