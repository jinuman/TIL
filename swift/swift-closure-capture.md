# Capturing Values

- 클로저는 선언 시점과 실행 시점이 다를 수 있다.
- 그러므로 클로저 외부 변수를 참조 하려면 그 변수에 대한 참조를 클로저가 내부적으로 가지고 있어야 된다.
- 이것을 클로저가 capture 한다고 한다.
- 클로저는 자신 주변에 있는 자신이 관여하는 변수들을 **생성 시점에** 저장(capture)한다.
    - 그러므로 클로저가 만들어질 때 마다 개별 메모리를 가진다.

### Example

```Swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var total = 0
    func incrementer() -> Int {
        total += amount
        return total
    }
    return incrementer
}

// incrementByTen 이 makeIncrementer 가 return 한 클로저가 캡쳐한 것을 가리키게 된다.
let incrementByTen = makeIncrementer(forIncrement: 10)

// 위에 makeIncremneter 함수가 종료한 후에 호출해도 문제 없다.
// 왜냐하면 makeIncrementer 가 return 한 메모리를 캡쳐한다.
print(incrementByTen())  // 10
print(incrementByTen())  // 20

// 클로저가 하나 더 만들어진 것이다. 그러므로 개별 메모리를 가진다.
let incrementByFive = makeIncrementer(forIncrement: 5)

print(incrementByFive())  // 5
```

- incrementByTen 은 클로저 타입이다.
- makeIncrementer 가 호출되는 시점에 함수를 return 하면서 incrementByTen 이 그걸 "캡쳐"한 것이다.
- 생성 시점에 캡쳐되기 때문에 incrementByFive 와 incrementByTen 은 각각 다른 메모리를 가리킨다.

## Closures are Reference types

```Swift
let copycat = incrementByTen
print(copycat())  // 30
```

- Closure 는 `reference type` 이다.

## Closure escaping

- `parameter` 로 전달된 **closure 는 함수 외부로 나갈 수 없다.**
    - 기본 옵션이 `@nonescaping` 이다.
- `@escaping` 키워드를 쓰면 가능하다.

## Auto Closure

> `@autoclosure`

- { } 로 감싸주지 않아도 자동으로 클로저를 만들어주는 키워드이다.

```Swift
var todoList: [() -> String] = []
var works: [String] = ["swift", "kotlin", "nodejs"]

func pushWork(work: @autoclosure @escaping () -> String) {
    todoList.append(work)
}

pushWork(work: works.remove(at: 0))
pushWork(work: works.remove(at: 0))

print(works.count)  // 3
for work in todoList {
    print(work())
}
```

- 클로저는 생성될 때가 아니라 호출될 때 실행된다.
- 그러므로 두번의 pushWork 함수 호출에서 works 는 수정되지 않는다.
- todoList 는 () → String 형태를 가지는 closure 배열이기 때문에 remove 뿐 아니라, 같은 형태를 가지는 모든 작업을 저장할 수 있다.