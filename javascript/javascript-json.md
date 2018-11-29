# JSON

JSON이란?
--
> **J**ava**S**cript **O**bject **N**otation

- 네트워크를 통해 데이터를 주고 받는 데 자주 사용되는 **경량의 데이터 형식**이다.
- 즉, 한마디로 **경량의 DATA-교환 형식**

자세히 알고싶다.. [JSON 이론](http://www.json.org/json-ko.html)  

### JSON은 name - value 형태의 쌍으로 이루어져 있다!  
- **주의할 점**: <u>`name`부분은 _**무조건 String**_ !!!</u>
- `value`부분은 기본 자료형, 배열, 객체이고, 각 쌍들은 쉼표(,)로 구분된다.

#### `{ ... }` : 객체(Object)라는 의미이다.
- 배열 안에 객체가 들어갈 수도 있다.

JSON Example
--
```JavaScript
{
  "person":[
     {
       "name": "Jinwoo",
       "gender": 1
       "employed": "No"
     },
     {
       "name": "Boyoung",
       "gender": 2
       "employed": "Yes"
     }
  ]
}    
```
위 json 구조는 `{ "person" : [ {}, {} ] }`형식이다.  
Swift 타입으로는 `[String: Any]`가 된다.
```JavaScript
[
  {
    "name": "Jinwoo"
    "age": 29
  },
  {
    "name": "Seowoo"
    "age": 25
  },
  {
    "name": "Boyoung"
    "age": 29
  }
]
```
위 json 구조는 `[{}, {}, {}]`형식이다.  
Swift 타입으로는 `[[String: Any]]`가 된다.



