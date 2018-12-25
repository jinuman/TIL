# Auto-Layout Basic
Auto-Layout
--

> View들 간의 관계를 이용하여 View의 위치와 크기를 자동으로 결정하는 Layout System

- view1.attr1 = view2.attr2 * multiplier + constant
  - view2는 없을 경우 nil이 될 수 있다.
  - 일차방정식 공식에 따라 레이아웃을 잡아가는 것  
- 목표값 = 비율 * 기준값 + 상수   
- First Item =(Relation) Multiplier * Second Item + Constant  

##### 폰트를 크게하고 그에 맞춰서 Object의 크기를 늘리고 싶을 때
- Menu -> Editor -> Size to fit Content
- Shortcut : `cmd + =`

Constraint
--
- Menu -> Editor -> Update Frames
  - Constraints 값에 맞게 Frame 재조정
  - 실수로 조정했을 때 다시 되돌아가는 기능
  - Shortcut : `cmd + option + =`
- Update Constraint Constants
  - 내가 마우스로 조정한 것으로 Constraints 변경

#### 크기가 있는 것과 없는 것

- 크기가 있는 것 : Label, ...
- 크기가 없는 것 : 안에 아무것도 없는 View, ... 
  - 따라서 크기 지정이 필요하다.

#### 같은 높이(height), 같은 넓이(width) 만들기
##### View 안에 두 개의 Object의 높이 혹은 넓이를 같게 해주고 싶을 경우?  

1. AutoLayout을 통해 넓이 혹은 높이가 지정된 Object를 선택  
2. 다른 Object로 드래그한 후 Equal Widths, Equal Heights 선택


Multiplier
--
- 보통은 처음 선택한 것이 FirstItem, 드래그 후 선택한 것이 SecondItem이 된다.
- 주의) 하지만 constant 값을 양수로 해주기 위해 xcode가 임의로 변경할 때도 있다.
- **Multiplier에선 1이 중앙이다.** 
- 왼쪽 위가 기준이다. (x, y) = (0, 0)


#### View 안에 3개의 View를 1 : 2 : 3 만들기

Equal Width 기준

1. First Item : 첫 번째 View, Second Item : 두 번째 View, Multiplier = 1:2 설정  
2. First Item : 첫 번째 View, Second Item : 세 번째 View, Multiplier = 1:3 설정


Alignment
--
_쉽게 설명_
- Horizontally in Container : Object의 너비를 중앙에
- Vertically in Container : Object의 높이를 중앙에

#### 가운데 정렬하기

1. Horizontally in Container (check)
2. Vertically in Container (check)

`CenterX = Center horizontally (x축)`  
`CenterY = Center vertically   (y축)`


#### View에 걸쳐있는 Object를 정렬하기

<u>부모 View를 넘어가는 Object의 위치지정은 Clip to Bounds를 하면 짤리기 때문에 `동등레벨`로 놓고 정렬해야 한다.</u>

- 참고) Storyboard -> Attributes inspector -> Clip to Bounds 

#### Button 왼쪽에 이미지를 넣었을 때, Button Title을 그만큼 오른쪽으로 이동하기

- Button -> Size inspector -> Content insets 에서 이미지의 너비 + Constraints 만큼 Setting..


Priority
--

- 상황에 따라 Content의 크기가 변해야 할 경우 적용한다.
- Calculator처럼 딱딱 크기가 일정하게 정해져 있는 경우엔 사용하지 않아도 된다.

### Content Hugging Priority
- Content Hugging : Self 의 크기가 유지된다는 의미이다.  
- Priority : **높은 값이 우선이다.**
- 정리하면 Priority 값이 높은 Content의 크기가 유지되고 다른 것들이 변경됨
- `같거나 늘어나는 경우`에 이 옵션을 쓴다. _"누가 늘어날래?"_  
- cf ) **Constraint에도 Priority가 있는데 낮은 것이 변한다는 개념은 똑같다.**

##### example)
label 2개가 수평으로 놓여져 있고 둘 사이 가운데 space constant를 좁게 설정한 상황일 때..
- 어느 label의 크기를 유지시키고, 어느 label의 크기를 변화시킬 지 결정할 때, 유지시키고 싶은 것의 priority 값을 높여주면 된다.

### Content Compression resistance priority

- `줄어드는 경우`에 이 옵션을 쓴다. _"누가 줄어들래?"_  
- 그 외에는 Hugging Priority랑 개념이 똑같다. 

