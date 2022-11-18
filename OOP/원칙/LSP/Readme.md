# 본론

오늘은 객체지향설계 5원칙 중 하나인 ISP에 관해서 알아보도록 하겠습니다.

## 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)

1988년 바바라 리스코프가 올바른 상속 관계의 특징을 정의하기 위해 발표한 것으로, 하위 타입은 상위 타입을 대체할 수 있어야 한다는 것을 말합니다. 즉, 해당 객체를 사용하는 클라이언트는 상위 타입이 하위 타입으로 변경되어도, 차이점을 인식하지 못한 채 상위 타입의 퍼블릭 인터페이스를 통해 서브 클래스를 사용할 수 있어야 한다는 것을 말합니다.

![LSP](./LSP.png)
--> 한줄 다이어그램 설명

```java
@Getter
@Setter
@AllArgsConstructor
public class Rectangle {

    private int width, height;

    public int getArea() {
        return width * height;
    }

}

public class Square extends Rectangle {

    public Square(int size) {
        super(size, size);
    }

    @Override
    public void setWidth(int width) {
        super.setWidth(width);
        super.setHeight(width);
    }

    @Override
    public void setHeight(int height) {
        super.setWidth(height);
        super.setHeight(height);
    }
}
```

```java
public void resize(Rectangle rectangle, int width, int height) {
    rectangle.setWidth(width);
    rectangle.setHeight(height);
    if (rectangle.getWidth() != width && rectangle.getHeight() != height) {
        throw new IllegalStateException();
    }
}
```

문제는 resize()의 파라미터로 정사각형인 Square이 전달되는 경우다. Rectangle은 Square의 부모 클래스이므로 Square 역시 전달이 가능한데, Square는 가로와 세로가 모두 동일하게 설정되므로 예를 들어 다음과 같은 메소드를 호출하면 문제가 발생할 것입니다.

```java
Rectangle rectangle = new Square();
resize(rectangle, 100, 150);
```

이러한 케이스는 명백히 클라이언트의 관점에서 부모 클래스와 자식 클래스의 행동이 호환되지 않으므로 리스코프 치환 원칙을 위반하는 경우입니다. 리스코프 치환 원칙이 성립한다는 것은 자식 클래스가 부모 클래스 대신 사용될 수 있어야 하기 때문입니다.

# 결론

짧게 LSP에 관해서 알아보았습니다. 읽어주셔서 감사합니다.

# 참고한 사이트

[https://mangkyu.tistory.com/194](https://mangkyu.tistory.com/194)
