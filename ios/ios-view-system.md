# View System

## What is View and Window?

- iOS 에서 화면에 애플리케이션의 콘텐츠를 나타내기 위해 Window 와 View 를 사용한다.
- Window 는 그 자체로 콘텐츠를 표현할 수 없지만 애플리케이션의 View 를 위한 컨테이너 역할을 한다.
- UIView 클래스 또는 하위 클래스의 인스턴스로 윈도우의 한 영역에서 콘텐츠를 표현한다.
- VIew 는 GestureRecognizer 를 사용하거나, 직접 터치 이벤트를 처리할 수 있다.

## View Hierarchy

> *View Hierarchy 에서 Parent View 는 Child View 의 위치와 크기를 관리한다.*

- 하나의 View 가 다른 View 를 포함할 때, 두 View 사이에는 부모 - 자식 관계가 형성된다.
- **iOS Class Hierarchy**

![ios-hierarchy](https://user-images.githubusercontent.com/26243835/55944527-c198d280-5c83-11e9-931a-48f14a153961.jpg)

![class-hierarchy](https://user-images.githubusercontent.com/26243835/55944544-c8274a00-5c83-11e9-8df0-d2389fc2c2a2.png)


### NSObject ( iOS root class)

> The root class of most Objective-C class hierarchies, from which subclasses inherit a basic interface to the runtime system and the ability to behave as Objective-C objects.

### UIResponder

> An abstract interface for responding to and handling events.

- UI의 모든 이벤트에 관여되어 있다.

    touchesBegan, touchesMoved, touchesEnded, 등등

- **UIView는 UIResponder를 상속받는다.**

## View 계층의 생성과 관리

- addSubview(_:)
- removeFromSuperview()
- SubView 를 SuperView 중간에 삽입할 수도 있다.

    insertSubview(_:at:)

- SuperView 내에 이미 존재하는 SubView 를 정렬할 수도 있다.

    bringSubview(toFront:)

    sendSubview(toBack:)