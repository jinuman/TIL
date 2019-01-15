# Auto-Layout Advanced

## Rotate

- Portrait(세로모드), Landscape(가로모드)에 따라 다르게 보여줄 수 있다.

1. Interface Builder에서 모드를 선택하고 `Vary for traits` 버튼을 누른다.
2. `height`, `width`에 따라 적용되는 모델들을 확인하고 변경될 Auto-layout 지정
3. `Done Varying`을 누르면 해당 모드에서 적용된다.

- 만약 Portrait 에서는 안보여주고, Landscape 에서만 보여주고 싶은 부분이 있을 경우
   1. Portrait 부분에서 보여주고 싶지 않은 부분 클릭
   2. Attributes inspector -> Add customization -> Add Variation 
   3. Installed 클릭 된 부분을 해제하면 된다.

## ScrollView

- UIScrollView의 하위 뷰로 UIView 추가하고 안에 Contents들을 집어 넣는다.
- 아래로 스크롤되면서 보여주게 하고 싶다면 UIView의 Constraints를 ScrollView에 맞추고,
- Bottom Constraint의 priority 값을 다른 Priority 값보다 낮추면 Contents의 사이즈에 따라 아래로 유동적으로 변한다.

## Dynamic Text

### Label을 Image에 배치할 때

- 비율에 따라 배치를 해야 Device의 크기가 변경이 되도 똑같이 보이게 할 수 있다.

1. Label과 이미지를 동시에 클릭하고 상하좌우 정렬을 한다.
2. 상황에 따라 필요 없는 정렬은 제거하고 수평 정렬, 수직 정렬로 Label의 위치를 잡아준다.
3. 보통 **bottom align의 multiplier 값을 변경 시키면서 이미지에서 Label의 위치를 잡는다.**
   - <u>위에서 아래로 정렬을 내려야 할 경우에도 아래쪽 정렬에 multiplier 값을 1 이상으로 변경시켜주면서 작업하면 된다.</u>

### Label의 Font 크기를 Device에 따라 다르게 표현할 때

1. 먼저 작업을 제일 큰 화면에서 한다.
2. **Autoshrink -> Minimum Font Scale** 을 0.5 정도로 지정한다.
   - Label의 크기가 작아질 때마다 크기를 작게 하는데 반 정도까지 작게 한다는 의미이다. 
3. Label과 같이 움직이는 Container와 Aspect Ratio 값을 잡아준다.
   - Label 클릭 후 Container로 드래그 후 Aspect Ratio 클릭
   - Label은 고정된 크기를 가지지만, Label 크기의 비율을 Container의 비율과 같이 가겠다는 의미이다.
- ✻ 만약 여러 줄의 Label을 표현해야 한다면 Lines = 0 해준다.
  - 만약 여러 줄을 Fix 하게 보여주면서 Device에 따라 크기가 변경되어야 할 땐 Lines 를 specific하게 지정해줘야 한다.



