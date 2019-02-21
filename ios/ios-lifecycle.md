# Life Cycle

## iOS class Hierarchy
![ios-hierarchy](https://user-images.githubusercontent.com/26243835/50977994-c7710180-1536-11e9-8970-aaa9ccb333d0.jpg)

#### NSObject ( iOS root class)
> The root class of most Objective-C class hierarchies, from which subclasses inherit a basic interface to the runtime system and the ability to behave as Objective-C objects.  

#### UIResponder
> An abstract interface for responding to and handling events.

- UI의 모든 이벤트에 관여되어 있다. 
  - ex) touchesBegan, touchesMoved, touchesEnded, ...
- UIView는 UIResponder를 상속받는다.

#### 바깥 부분을 Tap 하면 키보드 내리는 코드

- 단순히 바깥 부분을 탭할 시 키보드 내리는 것만 가능하다. 간단하게는 이렇게 사용

```Swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
  self.view.endEditing(true)
}
```

- Gesture Recognizer 이용하는 방법

```Swift
// viewDidLoad 에서 제스쳐 추가 및 델리게이트 설정하고 밑에 함수를 구현해준다.

let tapGesture = UITapGestureRecognizer()
tapGesture.delegate = self
view.addGestureRecognizer(tapGesture)

extension ViewController: UIGestureRecognizerDelegate {
    // 키보드 내리기
    func gestureRecognizer(_ gestureRecognizer: UIGestureRecognizer, shouldReceive touch: UITouch) -> Bool {
        self.view.endEditing(true)
        // editing 종료 후 상태를 검사하는 코드를 주로 작성 ex. checkIfAllDone()
        return true
    }
}
```

* * *
## App Life Cycle

- UIKit apps are always in one of five states, which are shown below.  
![image](https://user-images.githubusercontent.com/26243835/49029380-c9ca0700-f1e7-11e8-91b8-477d2f3f1d30.png)

- Inactive와 Active 상태를 **Foreground** 라고 말한다.

### AppDelegate methods
- application(didFinishLaunchingWithOptions)
  - **처음에 한번만 호출됨**
  - 그러므로 최초 초기화 코드에 사용됨
- applicationDidBecomeActive
  - inactive -> active 
  - 게임 앱에서 "다시 할래요?" 같은 것들을 넣어 주는 곳
- applicationWillResignActive 
  - active -> inactive
  - ex) 홈버튼 두번 눌러서 전환창
  - ex) 문자나 전화가 올 때 화면 위에 뜨는 것 같이 잠시 인터럽트를 주는 코드를 여기에 배치
- applicationDidEnterBackground
  - inactive -> background
  - ex) 홈 버튼 한번 눌러서 홈 화면 보기
- applicationWillEnterForeground
  - background -> active
- applicationWillTerminate
  - 앱이 종료되기 직전에 호출됨


#### UIApplicationDelegate Flow! 

- 처음 시작시!
  - didFinishLaunchingWithOptions -> applicationDidBecomeActive
- 홈 버튼을 한번 누르면!
  - applicationWillResignActive -> applicationDidEnterBackground
- 그리고 다시 앱으로 들어가면!
  - applicationWillEnterForeground -> applicationDidBecomeActive
- 홈 버튼 두번 누르고 앱 종료!
  - applicationWillResignActive -> applicationDidEnterBackground -> applicationWillTerminate

**기본적으로 실행되는 앱이 우선이고 리소스를 다 쓰게 되면 백그라운드에 있는 앱 중에 랜덤으로 날라가게 된다.**   
필요에 따라 background로 넘어갈 때 데이터들을 저장하고 다시 foreground가 될 때 데이터를 로딩 해야한다.

* * *

## ViewController Life Cycle
- viewDidLoad는 클래스 생성 시 1회 호출 된다.
- appear, disappear 관련된 메소드들은 여러번 호출될 수 있다.
- UIKit 프레임워크가 호출하는 함수이므로 직접 호출할 수 없다.
- 오버라이드 할 경우 반드시 super 구현을 호출해야 한다.

![image](https://user-images.githubusercontent.com/26243835/50978068-f1c2bf00-1536-11e9-8efb-6479c0c9bab8.png)
### View 상태 변화 Methods
> 뷰가 나타나거나 사라지는 등, 뷰가 화면에 보이는 상태가 변화할 때 호출되는 methods

##### func viewDidLoad()
- **해당 ViewController 클래스가 메모리에 처음 로딩될 때 1회 호출됨**
- 메모리 경고로 뷰가 사라지지 않는 이상 다시 호출되지 않음
- 일반적으로 리소스 초기화, 초기화면 구성에 쓰임

##### func viewWillAppear()

- 해당 ViewController가 나타나기 직전에 일어나야 할 일을 여기에 배치
- 뷰의 추가적인 초기화 작업을 하기 좋은 시점
  - ex) 내부 설정으로 뷰를 변경하고 변경된 뷰를 reload할 경우
- 다른 뷰로 이동했다가 되돌아오면 재호출됨

##### func viewDidAppear()

- 화면이 표시되면 호출되는 메소드
- 화면을 다 보여준 후 뷰에 추가적인 작업을 하기 좋은 시점
  - ex) 화면이 적용될 애니메이션을 그릴 때
  - ex) 화면을 다 보여준 후 키보드 띄우기

##### func viewWillDisappear()

- 뷰가 뷰 계층에서 사라지기 직전에 호출됨
- 뷰가 생성된 뒤 발생한 변화를 이전상태로 되돌리기 좋은 시점

##### func viewDidDisappear

- 뷰가 뷰 계층에서 사라진 후 호출되는 메소드
- 뷰를 숨기는 것과 관련된 추가적인 작업을 하기 좋은 시점
- 시간이 오래 걸리는 작업은 하지 않는 것이 좋다. 

### View Layout 변화 Methods
> 뷰가 생성된 후 바운드 및 위치 등의 레이아웃에 변화가 생겼을 때 호출되는 methods

##### func viewWillLayoutSubviews()

- 뷰가 서브뷰의 레이아웃을 변경하기 직전에 호출되는 메소드
- 서브뷰의 레이아웃을 변경하기 전에 수행할 작업을 하기 좋은 시점

##### func viewDidLayoutSubviews()

- 서브뷰의 레이아웃이 변경된 후 호출되는 메소드
- 서브뷰의 레이아웃을 변경 한 후 추가적인 작업을 수행하기 좋은 시점

✻ 그 외에도 다양한 메소드들이 있다.
#### ✻ 중요! <u>View Controller에서 위 메소드들을 사용하기 위해선 override 키워드를 명시하고 super를 호출해야 한다!</u>

* * * 
