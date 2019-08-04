# Lazy property

> 게으른 (저장) 프로퍼티

## Lazy Stored Property

> 처음 사용될 때 메모리에 값을 올린다.
그 이후부터는 계속해서 메모리에 올라온 값을 사용한다.

1. 값이 사용 되기 전까지 값이 계산되지 않는 프로퍼티이다.
    - 따라서 **computed property 는 lazy 로 사용할 수 없다.**
2. let은 초기화를 함과 동시에 값을 가져야 한다.
    - 따라서 `lazy`는 값이 필요할 때 초기화 하므로 let 으로 선언할 수 없다.
    - **반드시 var 와 함께 쓰여야 한다.**
3. 기본적으로 class, struct 에서만 사용 가능하다.
- 초기 값이 복잡하거나, 계산 비용이 많이 드는 상황에서 lazy var 를 사용하면 좋다.

```Swift
class File {
    var filename = "data.txt"
}

class Manager {
    lazy var file = File()
    var fileBook = [File]()
}

let manager = Manager()
manager.fileBook.append(File())
let file2: File = manager.file  // lazy var로 선언된 file은 생성할 때 초기화되지 않고 여기에서 처음으로 초기화된다.
file2.filename = "newData.txt"
manager.fileBook.append(file2)
print("\(manager.fileBook[0].filename), \(manager.fileBook[1].filename)")
// data.txt, newData.txt
```

### Lazy and Closure

- lazy 에 어떤 특별한 연산을 통해 값을 넣어 주기 위해서는 코드 실행 블록인 `closure`를 사용한다.
- class, struct 의 다른 프로퍼티의 값을 lazy 변수에서 사용하기 위해서는 closure 내에서 self 를 통해 접근이 가능하다.
    - 기본적으로 일반 변수들은 class 가 생성된 이후에 접근이 가능하기 때문에 class 내의 다른 영역(메소드, 일반 프로퍼티)에서 *self를 통해 접근할 수 없지만,* **lazy 키워드가 붙으면 생성 후 추후에 접근할 것이라는 의미이기 때문에 closure 내에서 self 로 접근이 가능하다.**

```Swift
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
    lazy var greeting: String = {
        return "Hello my name is \(self.name)"
    }()
}

let handsomePerson = Person(name: "Jinwoo")
print(handsomePerson.greeting) // Hello my name is Jinwoo
handsomePerson.name = "Minwoo"
print(handsomePerson.greeting) // Hello my name is Jinwoo
```

- 위에서 handsomePerson.greeting 은 closure 의 실행의 결과를 담은 것이다.
- 클래스 내부의 클로저에서 객체를 self 로 참조한다면 메모리 누수가 생길 수 있지만, 위는 ()를 통해 즉시 실행 후 결과를 돌려주고 끝내기 때문에, 메모리 누수 걱정이 없다.
- 또한 프로퍼티를 “Minwoo”로 변경해도 처음 사용할 때 메모리에는 “Jinwoo”가 올라가 있기 때문에 “Jinwoo”가 출력된다.
    - Immediately invoked closure 이기 때문이다.
- **✻** **lazy var 프로퍼티는 직접 접근해서 변경하지 않는 한 바뀌지 않는다.**
    - **직접 접근하지 않는 한, 처음 불린 그 상태를 유지한다.**
    - For Example
- lazy 변수가 `closure`라면 얘기가 다르다. 헷갈리지 말자.

```Swift
class Person {
    var name:String
    lazy var greeting: () -> String = { [weak self] in
        guard 
            let self = self,
            let name = self.name else { return "fail" }
        return "Hello my name is \(name)"
    }
    init(name:String) {
        self.name = name
    }
}

var handsomePerson = Person(name:"Jinwoo")
print(handsomePerson.greeting()) // Hello my name is Jinwoo
handsomePerson.name = "Minwoo"
print(handsomePerson.greeting()) // Hello my name is Minwoo
```

- 만일 위처럼 greeting 이 클로저의 실행 결과를 담은 것이 아닌 클로저 자체를 담고 있는 변수 라면 반드시 [weak self] 를 통해 메모리 누수를 방지 해야 한다.
- 값이 아닌 클로저 자체가 메모리에 올라가 있고, 클로저 내부에서 클래스 프로퍼티를 참조하고 있기 때문에, name 을 변경했을 때 처음 불린 상태인 Jinwoo 가 출력 되는 것이 아닌 Minwoo 로 변경된다.