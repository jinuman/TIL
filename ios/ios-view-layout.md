# View Layout

## View, ViewController Layout cycle
> 퍼포먼스가 좋은 앱 개발을 위해 반드시 알아야 하는 개념이다.

![view-layout-cycle](https://user-images.githubusercontent.com/26243835/57640191-b8858300-75ec-11e9-8386-69cf59102bc3.jpeg)

### 대략적으로 3단계이다.

- Step 1. Constraints ( 제약 )

    이 단계에서 Auto-layout 의 제약을 갱신한다.
    Constraints 갱신은 subview 부터 superview의 순서로 호출된다.
	```Swift
        override func updateConstraints() {
            // 제약을 갱신하는 코드를 여기에 적는다.
            // 아직 레이아웃을 실행하는 setNeedsLayout, layoutIfNeeded 는 호출하면 안된다.
            // 또한, setNeedsDisplay 도 호출하면 안된다.
            
            // super는 가장 마지막에 호출한다.
            super.updateConstraints()
        }
	```
- Step 2. Layout

    제약을 바탕으로 레이아웃을 실행한다.
    Layout 갱신은 반대로 superview 부터 subview 순서로 호출된다.

- Step 3. Draw ( 그리기 )

    Step 2 의 Layout 후 UIView 의 drawRect(rect: CGRect) 가 호출된다.
    이 스텝에서 필요하다면 `CoreGraphics` 를 사용하여 그리게 된다.

# Layout

## Main run loop
> Application 이 실행되면 UIApplication 의 main thread 에서 main run loop 를 실행시킨다.

![update_cycle](https://user-images.githubusercontent.com/26243835/57640276-026e6900-75ed-11e9-874f-cbcc1e28a731.png)

- **Update Cycle 에서 재계산된 View 의 position 이나 layout 을 적용시킨다.**

## layoutSubviews

> The default implementation uses any constraints you have set to determine the size and position of any subviews.

- 필요하면 override 해서 사용할 수 있다.
- You should override this method only if the autoresizing and constraint-based behaviors of the subviews do not offer the behavior you want.

## setNeedsLayout()

- layoutSubviews() 를 `update cycle` 에서 호출 해달라고 예약하는 메소드이다.
- 예약하는 행위이므로 비동기적인 동작이다.
- View 의 그려지는 모습은 `update cycle` 에 들어가게 되면 바뀌게 된다.

## layoutIfNeeded()

- layoutSubviews() 를 동기적으로 호출 즉시 실행시킨다.
- `update cycle` 까지 기다리지 않고 그 즉시 View 를 그리는 것이다.
- **그러므로 Animation 효과를 보고 싶을 때 주로 사용된다.**