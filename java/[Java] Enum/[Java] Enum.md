# 본론

오늘은 Java의 Enum 클래스를 활용하는 방법에 관해서 알아보도록 하겠습니다.

### Enum이 없던 시절

Java Enum 타입은 일정 개수의 상수 값을 정의하고, 그 외의 값은 허용하지 않습니다. 과거에는 특정 상수 값을 사용하기 위해선 모두 상수로 선언해서 사용했습니다.

```java
public static final String MON = "Monday";
public static final String TUE = "Tuesday";
public static final String WED = "Wednesday";
```

이렇게 사용하면 개발자가 실수하기도 쉽고 한 눈에 알아보기도 쉽지 않습니다. 그리고 관련 있는 값들끼리 묶으려면 접두어를 사용해서 점점 변수명도 지저분해집니다.

### Enum

아래처럼 단순하게 요일을 열거한 Enum 클래스를 만들 수 있습니다.

```java
public enum Day {
    MON, TUE, WED, THU, FRI, SAT, SUN
}
```

하지만 각각의 요소들이 특정한 값을 갖게 하고 싶을 수도 있습니다. 예를 들어, 각 요일의 풀네임이 필요할 때도 있을 것입니다.

### 생성자와 final 필드 추가

```java
public enum Day {
    MON("Monday"),
    TUE("Tuesday"),
    WED("Wednesday"),
    THU("Thursday"),
    FRI("Friday"),
    SAT("Saturday"),
    SUN("Sunday");

    private final String label;

    Day(String label) {
        this.label = label;
    }

    public String label() {
        return label;
    }
}
```

Enum 요소에 특정 값을 매핑하고 싶다면 위 코드처럼 필드 값을 추가하면 됩니다. 여기서는 `label`이란 String 값을 추가했습니다. 필드 값을 추가하면 생성자도 함께 추가해줘야 하는데, Enum 클래스는 생성자가 있다고 하더라도 `new` 연산으로 생성할 수 없습니다.

```java
System.out.println(Day.MON.name());      // MON
System.out.println(Day.MON.label());     // Monday
```

이렇게 규칙이 존재하는 특정 요소들을 하나의 Enum 클래스로 묶어두면 가독성도 좋아지고 if 문으로 일일히 검사할 필요도 없어서 편리합니다. 필드 값을 추가할 때, 이름을 `name`으로 정하는 건 피하는 게 좋습니다. Enum 클래스 자체에서 `name()`이라는 메서드를 제공하기 때문에 헷갈릴 수 있습니다.

### 필드 값으로 Enum 값 찾기

Enum은 자체적으로 `name()` 값으로 Enum 값을 찾는 `valueOf()` 메소드를 제공합니다. 특정 필드 값으로 찾는 기능은 제공하지 않기 때문에 직접 만들어야 합니다.

### 직접 Enum values() 순회하며 찾기

```java
public enum Day {
    // ..codes

    public static Day valueOfLabel(String label) {
        return Arrays.stream(values())
                    .filter(value -> value.label.equals(label))
                    .findAny()
                    .orElse(null);
    }
}
```

다른 필드 값으로 찾기 위해선 위 코드와 같이 모든 Enum 값을 순회하면서 일치하는 값이 있는지 찾아야 합니다.

### Enum Converter의 역할

# 참고한 사이트

[https://bcp0109.tistory.com/334](https://bcp0109.tistory.com/334)
[https://techblog.woowahan.com/2527/](https://techblog.woowahan.com/2527/)
[https://techblog.woowahan.com/2595/](https://techblog.woowahan.com/2595/)
[https://techblog.woowahan.com/2600/](https://techblog.woowahan.com/2600/)
[https://minji6119.tistory.com/42](https://minji6119.tistory.com/42)
