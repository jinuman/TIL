# Coordinate System

> View 의 좌표계

- 좌측 상단 모서리를 원점 `origin(0, 0)` 으로 하며, 원점으로 부터 아래쪽, 오른쪽 방향이 + 방향이다.
- 좌표 값은 해상도와 상관없이 콘텐츠의 위치를 잡는 부동 소수점을 사용하여 나타낸다.

## View Rectangle

- 어디에 그려야 할 지 `위치`를 알아야 한다.
- 어떤 `크기`로 그려야 할지를 알아야 한다.
- `CGRect` 는 사각형의 크기와 위치에 대한 정보를 담고 있다.

![View-rect](https://user-images.githubusercontent.com/26243835/55944974-c611bb00-5c84-11e9-847d-f27683b4452c.jpg)

## Frame

> The frame rectangle, which describes the view's location and size in its **superview's coordinate system.**

- `SuperView` 의 좌표 시스템 안에서 위치와 크기를 나타낸다.
- 자신의 `SuperView' origin` 으로 부터 떨어진 곳에 사각형을 그린다.

## Bounds

> The bounds rectangle, which describes the view's location and size in its own coordinate system.

- 자신만의 좌표 시스템 안에서 나타낸다.
- 자신만의 좌표계를 기준으로 위치 (x, y) 및 크기 (width, height) 로 표현되는 사각형이다.
- *주로 UI 변경 후 View 의 크기를 알고 싶을 때 사용한다.*
- 밑에서 redView 와 blueView 의 bounds.origin x, y 좌표는 둘다 (0, 0) 이다.

![bounds](https://user-images.githubusercontent.com/26243835/55945129-26a0f800-5c85-11e9-97e6-61cd36ff117e.gif)

```Swift
redView.bounds.origin.x = 60
redView.bounds.origin.y = 50
```

- **Bounds 를 변경하는 것은 해당 위치에서 View 를 다시 그리라는 의미이다!**

### Bounds 는 ScrollView 의 핵심이다.

<img width="338" alt="scrollView-bounds" src="https://user-images.githubusercontent.com/26243835/55945185-3f111280-5c85-11e9-9ae6-08ff8ae1f921.png">

- 위 그림은 우리 눈에는 ScrollView 가 움직이는 것처럼 보이지만, 실제로는 ScrollView bounds 의 x 를 양수 값으로 준 것이다.

## Frame vs Bounds

- Frame

```Swift
scrollView.frame.origin.x = 100
scrollView.frame.origin.y = 100
```

![frame](https://user-images.githubusercontent.com/26243835/55945241-6071fe80-5c85-11e9-9671-67c660b75fa5.png)

- Bounds

```Swift
scrollView.bounds.origin.x = 100
scrollView.bounds.origin.y = 100
```

<img width="410" alt="bounds" src="https://user-images.githubusercontent.com/26243835/55945250-64058580-5c85-11e9-9d31-2e46edcf2065.png">

## Bounds 를 꼭 써야하는 상황

- transform 을 scale up 하거나 rotation 한 다음 원 상태로 복귀하는 상황에서 frame.size 는 변하는데 bounds.size 는 변하지 않는다.
- 그러므로 transform 상황에서 view 의 크기를 구할 때는 bounds 를 쓰는 게 맞다.