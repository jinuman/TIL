# Property
> Swift 에는 3가지 종류의 프로퍼티가 있다.

1. 저장 프로퍼티 ( Stored Property )
2. 연산 프로퍼티 ( Computed Property )
3. 타입 프로퍼티 ( Type Property )

## Stored Property

- class와 struct 에서만 사용된다.
- 상수( let ), 변수( var )값을 인스턴스의 일부로 저장

✻ 참고: 구조체는 기본적으로 저장 프로퍼티들을 파라미터로 가지는 이니셜라이져가 있다.  
✻ 참고: 구조체는 Value Type, 클래스는 Reference Type  
```Swift
class PersonClass {
    let name: String
    var nameOfJob: String?
    
    init(name: String, nameOfJob: String?) {
        self.name = name
        self.nameOfJob = nameOfJob
    }
}

struct PersonStruct {
    let name: String
    var nameOfJob: String?
}

let person1 = PersonClass(name: "jinuman", nameOfJob: nil)
person1.name = "jinu" // error: 'name' is a 'let' constant
person1.nameOfJob = "Developer"
if let job = person1.nameOfJob {
        print(job)  // "Developer"
}

let person2 = PersonStruct(name: "jinuman", nameOfJob: nil)
person2.name = "jinu" // error: 'name' is a 'let' constant
person2.nameOfJob = "Developer" // error: 'person2' is a 'let' constant
```

## Computed Property

- class, struct, enum 에 사용된다.
- 반드시 **var**로 선언되어야 한다.
- Setter를 줄이면 반드시 `newValue`키워드를 사용해야 한다.
- set만 구현 **X**
```Swift
set(anyKeywordIsOK) {
  x = anyKeyworkIsOk * 2
}
// 줄이면..
set { 
  x = newValue * 2
}
```
- get만 있을 때는, get을 "생략"해도 된다.
- 연산 프로퍼티는 값을 저장하지 않는다!!
```Swift
class Point {
    var storedX: Int = 1
    var x: Int {
        get {
            return storedX
        }
        set {
            storedX = newValue * 2
        }
    }
}
var p: Point = Point()
p.x = 12
/* 
위 코드는 newValue에 12가 들어가고,   
storedX에는 12 * 2 = 24가 저장된다.  
만약 값을 읽으면 storedX에 있는 값을 리턴하고,   
값을 지정하면 2배를 해서 storedX에 할당한다.  
* /
```

## Type Property

- 프로퍼티를 `타입 자체`와 연결할 수 있는 것이 타입 프로퍼티이다.
- `static`키워드를 사용하여 정의할 수 있다.
- 저장 타입 프로퍼티, 연산 타입 프로퍼티가 있다.
- 연산 타입 프로퍼티의 경우 `class`키워드를 사용하여 SubClass가 SuperClass의 구현을 재정의(override) 할 수 있다.
- 타입 자체의 이름을 통해 프로퍼티에 접근하는 것 빼면, 위의 인스턴스 프로퍼티와 사용하는 법이 같다.
  - java에서 static이랑 비슷하게 생각하면 될 것 같다.


