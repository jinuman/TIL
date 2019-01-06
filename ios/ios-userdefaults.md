# UserDefaults
> 애플에서 크기가 작고 단순한 데이터들의 영속성을 위해 만들어 놓은 `데이터 저장소`

- Application을 종료해도 그대로 남아있다.  
- <u>사용자 설정</u> ( On/Off, 날짜 형식 선택 ) 같은 데이터를 저장하는 데 적합하다.
- Dictionary와 비슷하게 Key-Value 방식으로 데이터를 저장한다.
  - `Key는 String이다.`
- Bool, Int, Double, String, Date, Data, [Any], [String: Any] 등의 값을 저장할 수 있다.
- 위 타입에 해당하지 않는 것은 `Data 타입`으로 변환해 저장한다.


## UserDefaults 사용법

- 주로 싱글턴을 쓴다. 거의 대부분 .. 
```Swift
let userDefaults = UserDefaults.standard

@IBOutlet weak var switch: UISwitch!  // For practice
```
### Set ( Save )
```Swift
@IBAction func switchAction(_ sender: Any) {
  userDefaults.set(switch.isOn, forKey: "switchState")
  userDefaults.synchronize()  // 명시적으로 저장시키는 용도.. , 안해줘도 된다.
}
```

### Get
```Swift
switch.isOn = userDefaults.bool(forKey: "switchState")

let anyObject = userDefaults.object(forKey: "name") // Any?
let name = userDefaults.string(forKey: "name") // String?
```

### 기본값 등록하기
```Swift
userDefaults.register(["name": "Jinwoo"])
```