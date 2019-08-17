# Closure - Retain cycle

```Swift
class HTMLElement {
    let name: String
    let text: String

    lazy var asHTML: () -> String = { [unowned self] in
        return "<\(self.name)>\(self.text)<\(self.name)>"
    }

    lazy var asHTML2: String = {
        return "<\(self.name)>\(self.text)<\(self.name)>"
    }()

    init(name: String, text: String) {
        self.name = name
        self.text = text
    }

    deinit {
        print("HTMLElement \(name) is being deallocated!")
    }
}

/* Test */

// 1번 케이스
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "Thank you.")
print(paragraph?.asHTML())
paragraph = nil

// 2번 케이스
paragraph = HTMLElement(name: "div", text: "For kind review")
print(paragraph?.asHTML2)
paragraph = nil


/*
 1. asHTML 에서 [unowned self] 제거 시 retain cycle 발생
 2. asHTML2 에서 [unowned self] 제거 시 문제 없음
 */
```

- 위와 같은 결과가 나타난 이유는 asHTML2 는 생성과 동시에 실행되는 closure 이기 때문이다.
- 생성과 동시에 실행되는 closure 는 타입이 해당 closure 의 return 타입이기 때문에 문제 없다고 생각한다.