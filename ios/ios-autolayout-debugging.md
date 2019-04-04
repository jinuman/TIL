# Auto-layout Debugging
> Symbolic breakpoint 이용한다.

### Set Symbolic breakpoint

- Symbol : `UIViewAlertForUnsatisfiableConstraints`

- Action : Debugger Command 설정 후 밑에 명령어를 추가한다.
```
For Swift
expr -l objc++ -O -- [[UIWindow keyWindow] _autolayoutTrace]
```
- __`Ambiguous` 검색해서 문제되는 곳을 찾는다.__


### For more
- 문제되는 view 의 주소를 찾아서 색깔을 바꾸어서 찾을 수도 있다.
- unsafeBitCast 이용
```
expr -l Swift -- import UIKit
expr -l Swift -- unsafeBitCast(0x7f88a8cc2050, to: UIView.self).backgroundColor = UIColor.red
```

### unsafeBitCast
- Convert raw address into an object that can interact with.
- 메모리 주소는 알고 있는데, 프로퍼티에 접근하기가 어려울 때 유용하다. 
- 성공하면 임시 `$R4`같은 `임시 변수`에 저장되고, print 해보거나 변경할 수 있다.

- For example
```Swift
expr let bug1 = unsafeBitCast(0x7f88a8cc2050, UIView.self)
expr print($bug1)

expr $bug1.frame = CGRect(x: 0, y: 0, width: 64, height: 64)
expr print($bug1)  // frame 이 변경된다.
```

