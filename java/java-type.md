# Java Data Type

- 기본형(Primitive Type)과 참조형(Reference Type)이 있다.
- 일반적인 분류 ↓
```
Java Data Type
ㄴ Primitive Type  
    ㄴ Boolean Type(boolean)  
    ㄴ Numeric Type
        ㄴ Integral Type
            ㄴ Integer Type(short, int, long)
            ㄴ Floating Point Type(float, double)
        ㄴ Character Type(char)
ㄴ Reference Type
    ㄴ Class Type
    ㄴ Interface Type
    ㄴ Array Type
    ㄴ Enum Type
    ㄴ etc.
```

## Primitive Type
- 비 객체 타입이므로 `null` 값을 가질 수 없다. 
  - 기본형에 `null`을 넣고 싶다면 `Wrapper Class`를 사용한다. 
  - `Wrapper Class`: 기본형을 클래스로 감싼 형태
  - `Wrapper Class`들은 멤버 변수가 `final`로 선언 되어있다. 
  - 참고로 void의 Wrapper class인 Void도 존재한다.
- OS에 따라 자료형의 길이가 변하지 않는다.
- 기본적으로 모든 정수형 상수는 int형으로, 실수형 상수는 double형으로 표현 및 저장한다.

|| <center>예시</center> | <center>특징</center> |
| - | :- | :- |
| byte | 0, -127, 128 | -2^7 ~ 2^7-1 |
| short | -32768, 32767 | -2^15 ~ 2^15-1 |
| int | -2147483648, 2147483647 | -2^31 ~ 2^31 - 1 |
| long | -9223372036854775808 | 21억이 넘어가면 이걸 쓴다. |
| float | 1.6f, 0.5f | f를 끝에 붙여야 한다. |
| double | 123.4, 1.234e2 | 범위가 float보다 넓고 보통 이걸 쓴다. |
| boolean | true / false | 1bit |
| char    | 'A', 'B' | 16bits 유니코드 |
| String  | "Hello" | 메모리가 터질 때 까지 |

- String은 참조형이지만 기본형처럼 사용된다. 그리고 불변 객체이다. 
  - 일반적으로 기본형 비교는 `=`를 사용하지만,  String은 `equals()`메소드를 사용해야 한다.
- long 타입보다 큰 값이 필요하면 `BigInteger`를 사용한다.

## Reference Type
- `java.lang.Object`를 상속받는다. 
- 선언한 자료형이 기본형이 아니라면 참조형이다. 

```Java
class MyObject{
    private int value;
    MyObject(int value) {
        this.value = value;
    }
    int getValue() {
        return value;
    }
    void setValue(int value) {
        this.value = value;
    }
}

public class test {
    public static void main(String[] args) {
        MyObject a = new MyObject(2);
        MyObject b = new MyObject(4);
        System.out.println(a.getValue()); // 2
        a = b;
        System.out.println(a.getValue()); // 4
        b.setValue(6);
        System.out.println(a.getValue()); // 6
    }
}
```
- 위 코드에서 b 객체의 멤버 변수 값을 바꿨지만 a 객체도 같은 객체를 참조하기 때문에 동일한 값을 출력한다.
- a와 b라는 변수가 가지는 것은 실제 객체가 아닌 객체의 주소를 가진다고 생각하면 된다.




