# Struct

- Swift의 대부분의 데이터 타입들은 `struct`로 되어있다. Int, String, Double 등등
- 값 타입이다.   
```Swift
struct Sample {
    var mutableProperty: Int = 100 // 가변 프로퍼티
    let immutableProperty: Int = 100 // 불변 프로퍼티
    static var typeProperty: Int = 100 // 타입 프로퍼티
    
    // 인스턴스 메서드
    func instanceMethod() {
        print("instance method")
    }
    // 타입 메서드
    static func typeMethod() {
        print("type method")
    }
}
```
- **struct는 인스턴스를 만들 때 let으로 선언하면 가변 프로퍼티라도 변경을 할 수 없다.**
  - class는 참조 타입이기 때문에 변경이 가능하다. 
```Swift
let immutable: Sample = Sample()
immutable.mutableProperty = 200 // error!
```
### 구조체는 언제 사용하나?
- 연관된 몇몇의 값들을 모아서 하나의 데이터 타입으로 표현하고 싶을 때
- 다른 객체, 함수 등으로 전달될 때 **참조가 아닌 복사를 원할 때**
- 상속이 필요가 없을 때

##### 타입 프로퍼티, 타입 메서드
- 위에서 Sample이라는 타입 자체가 사용할 수 있는 프로퍼티나 메서드를 뜻한다.
- 그러므로 인스턴스를 만들지 않아도 된다.
- 인스턴스에서 타입 프로퍼티나 타입 메서드를 사용하는 것은 불가능하다.
```Swift
Sample.typeProperty = 200
Sample.typeMethod() // "type method"
immutable.typeMethod()  // error!
```
- 키워드는 \`  \`로 묶어주면 변수로 사용할 수 있다.
- **Struct는 Class와 다르게 init이 꼭 필요하지 않다.**
```Swift
struct Student {
    var name: String
    var `class`: String
}

let student = Student(name: "jinu", class: "A반")
print(student.name)     // "jinu"
print(student.class)    // "A반"
```


