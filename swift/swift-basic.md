# Super Basic

Swift의 특징
--
1. Swift : 정적 타입 언어, Objective-C : 동적 타입 언어
> 정적 타입 언어란 : 타입을 컴파일시 결정하는 언어

2. ARC( Automatic Reference Counting ) 이용한 메모리 관리
3. 멀티 패러다임 지원 - OOP, FP( 함수형 프로그래밍 )

Constants and Variables
--
The value of a constant can't be changed once it's set, whereas a variable can be set to a different value in the future.
```Swift
let constant: String = "차후에 변경이 불가능한 상수 let"
var variable: String = "차후에 변경이 가능한 변수 var"
```

Data Type
--
- Swift의 대부분의 데이터 타입들은 `struct`로 되어있다. Int, String, Double 등등
#### String

- Multiline String Literals  

```Swift 
var str = "Hello, playground.\nGoodbye Objective-C\n\"Hi Swift\""
var str2 = """
Hello, playground.
Goodbye Objective-C
"Hi Swift"
"""
str == str2
```
- 빈 변수 선언
```Swift
var emptyString = ""
var anotherEmptyString = String()
if anotherEmptyString.isEmpty {
    print("Nothing to see here")
}
```
#### Any
- Swift의 모든 타입을 지칭하는 키워드

#### AnyObject
- 모든 클래스 타입을 지칭하는 **프로토콜**

### nil
- 없음을 의미하는 키워드

Basic Operator
--
- a++, ++a 이런 증감연산자 (x), 그러므로 a += 1 해줘야 한다.
- **closed range operator** : 1...5
  - 1 부터 5까지
- **half-open range operator** : 1..<5 
  - 1 부터 4까지

Conditional Statements 
--
1. if-else
2. switch : 정수 외에 대부분의 기본 타입을 사용 가능
- Swift는 `if-else`문 보다 `switch`문이 더 강력하다.
- `switch`문에서 `break`는 꼭 필요하지 않다. 대신 `default`문은 필요하다.

if and switch
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
Control flow
--
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
- `stride` 함수를 이용하면 index 접근이 더 편하다.
  - through : 끝 포함, to : 끝 포함 (x)

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

* * * 

##### State ( 상태 ) : 프로그램 안에서 변할 수 있는 요소
- 정말 변해야 하는 것만 변할 수 있게 만들수록 프로그램은 단순해진다. 
- 최대한 상태를 많이 만들지 말자.
- 최대한 var 보다 let 을 쓰는 것이 앱의 성능이 좋아진다.

##### 주석 사용을 피해야 하는 이유
- 구현된 코드와 내용이 다른 주석은 오히려 독이다.
- 코드는 자주 변경되기 때문에 주석도 변경해주어야 한다.
- **주석 없이도 의도와 동작을 알 수 있도록 코드를 작성하는 것이 좋다. ( Show, don't tell )**
- 그럼에도 주석을 쓰는 경우 : 코드만으로는 다른 개발자와 협업에 있어서 이해하기 힘든 로직이 있을 때

##### 타입 추론
- 컴파일러가 좌항, 우항의 타입 방정식을 풀어서 적절한 타입을 계산
- 그러나 타입을 써주는 것이 성능상에도 좋고 협업상에도 좋다.

##### Type Aliases : 타입 별칭
```Swift
typealias StatusCode = Int  
// 위 처럼 선언하면 Int 와 StatusCode 를 서로 교환해가며 쓸 수 있다. 말그대로 별칭
```
##### 타입 출력하기
```Swift
print("variable : \(type(of: variable))")
```