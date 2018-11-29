# Life Cycle

### NSObject : 최상위 클래스

### UIResponder
> An abstract interface for responding to and handling events.

- UI의 모든 이벤트에 관여되어 있다. 
- ex) touchesBegan, touchesMoved, touchesEnded, ...
- UIView는 얘를 상속받는다.
#### 

#### View 부분을 Tab 하면 키보드 내리는 코드
```Swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
  self.view.endEditing(true)
}
```

App Life Cycle
--
#### xcode documentation 보면서 공부할 2곳
1. Using Responders and the Responder Chain to Handle Events
2. Managing Your App's life cycle

#### 최상위 객체 중 알아야 할 것 4가지 ( 정독 해볼 것 )
1. UIResponder
2. UIView
3. UIViewController
4. UIApplication

### 1. Using Responders and the Responder Chain to Handle Events
When your app receives an event, UIKit automatically directs that event to the most appropriate responder object, known as the `first responder`
#### Responder Chains in an app  
![image](https://user-images.githubusercontent.com/26243835/49028904-ceda8680-f1e6-11e8-97ce-10f89009dae2.png)
#### Determining an Event's First Responder
UIKit designates an object as the first responder to an event based on the type of that event. 
문서 참고할 것..

### 2. Managing Your App's life cycle
UIKit apps are always in one of five states, which are shown below.  
![image](https://user-images.githubusercontent.com/26243835/49029380-c9ca0700-f1e7-11e8-91b8-477d2f3f1d30.png)

AppDelegate.swift 
--
##### Methods
- application
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
##### UIApplicationDelegate Flow! 
**개발 시 영어 주석 꼭 읽어볼 것!**
- 처음 시작시!
  - didFinishLaunchingWithOptions -> applicationDidBecomeActive
- 홈 버튼을 한번 누르면!
  - applicationWillResignActive -> applicationDidEnterBackground
- 그리고 다시 앱으로 들어가면!
  - applicationWillEnterForeground -> applicationDidBecomeActive
- 홈 버튼 두번 누르고 앱 종료!
  - applicationWillResignActive -> applicationDidEnterBackground -> applicationWillTerminate
**기본적으로 실행되는 앱이 우선이고 리소스를 다 쓰게 되면 백그라운드에 있는 앱 중에 랜덤으로 날라가게 된다.**
그러므로 잘 만들어진 앱은 background로 넘어갈 때 데이터들을 저장하고 다시 foreground가 될 때 데이터를 땡겨와야(로딩해야)한다.

ViewController.swift
--
#### func viewDidLoad()
- **해당 ViewController 클래스가 생성될 때**
- 일반적으로 리소스 초기화, 초기화면 구성에 쓰임
- 클래스 생성 시 딱 1번만 호출됨

#### func viewWillAppear()
- 해당 ViewController가 나타나기 직전에 일어나야 할 일을 여기에 배치
- ex) 내부 설정으로 뷰를 변경하고 변경된 뷰를 reload할 경우

#### func viewDidAppear()
- ex) 화면이 적용될 애니메이션을 그릴 때
- ex) API로부터 정보를 받아와 업데이트 할 때?
- ex) 키보드 띄우기
  - 화면을 다 보여준 후 키보드를 띄워주는 UX를 구현하고 싶을 경우

#### func viewWill/DidAppear()
- 다른 ViewController로 전환 시 호출됨

