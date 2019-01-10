# Tuple
> 변경 불가능한 배열이라고 할 수 있다.
> Python의 Tuple과 매우 비슷하다.


- 배열과는 다르게 소괄호 `()` 이용해 아이템을 정의한다.
- **struct와 tuple은 동치이다.** 즉, 데이터 손실없이 서로 교환 가능하다.
- 자주 사용될 것 같으면 tuple보다 struct를 사용하는 것이 효율적이다.
- 간단하게 테스트용으로 구조체 대체가 가능하다.
```Swift
let tuple = (1, "second", 0.3)
let first = tuple.0
let second = tuple.1

// 변수, 상수 활용 가능
let (item1, item2, item3) = tuple
let (_, it, _) = tuple
print(item1)    // 1
print(it)       // "second"
```


### Named Tuple

- 개별 요소에 인덱스 대신 이름을 붙일 수도 있다.
```Swift
var namedHttpError = (code: 404, message: "Not Found")
namedHttpError = (500, "unknown")
print(namedHttpError.code)          // 500
print(namedHttpError.message)       // "unknown"
print(namedHttpError.0)             // 500
print(namedHttpError.1)             // "unknown"
```

### Multi-return

- 일반적으로 함수는 하나의 값만 리턴하는데, **2개 이상의 값을 return해야 할 경우**사용한다.
- 보통은 구조체나 클래스를 이용하는 경우가 다반사다.
```Swift
func plusAndMinus(a: Int, b: Int) -> (Int, Int) {
    return (a + b, a - b)
}

let (plusResult, minusResult) = plusAndMinus(1, 2)
```