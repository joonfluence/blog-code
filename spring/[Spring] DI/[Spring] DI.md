# 서론

# 본론

### 의존성 주입이란 

내부에서 직접 주입하는 것이 아니라 외부에서 주입을 해주는 것을 `의존성 주입`이라고 합니다. 주입이란 말은 외부에서라는 뜻을 내포하고 있는 단어입니다. 결국 자동차 내부에서 타이어를 생산하는 것이 아니라, 외부에서 생산된 타이어를 자동차에 장착하는 작업이 `주입`입니다. 

```java
public class Car {
    private Tire tire;

    public Car(Tire tire) {
        this.tire = tire;
    }
}
```

의존성 주입을 하는 까닭은 다음과 같습니다. 만약 아래의 경우처럼 Car 내부에서 타이어를 직접 생산한다면 타이어가 바뀔 때마다 코드의 수정이 계속 일어나야 할 것입니다. 

```java
public class Car {
    private Tire tire;

    public Car() {
        this.tire = new KoreaTire();
    }
}
```

반면 Car가 직접 Tire를 주입하는 것이 아니라 외부에서 주입을 받는다면 Car 클래스에는 수정이 일어나지 않아도 되고 따로 모듈화하여 뺄 수도 있습니다. 또한 외부에서 주입을 하면 디자인 패턴의 꽃이라고 할 수 있는 `전략패턴`을 사용할 수 있습니다.

```java
public class Client {
    public static void main(String[] args) {
        Tire tire = new KoreaTire();
        Car car = new Car(tire);
    }
}
```

### Setter를 통한 의존성 주입

생성자를 통해 의존성을 주입하면 한번 장착한 타이어는 더 이상 타이어를 교체할 수 없다는 문제점이 존재합니다. 운전자가 원할 때 타이어를 교체하고 싶다면 setter 의존성 주입을 사용해야 합니다. 아래와 같이 말이죠. 

```java
public class Car {
    private Tire tire;

    public Tire getTire() {
        return tire;
    }

    public void setTire(Tire tire) {
        this.tire = tire;
    }
}

public class Client {
    public static void main(String[] args) {
        Tire tire = new KoreaTire();
        Car car = new Car();
        car.setTire(tire);   // setter 의존성 주입
    }
}
```

### 필드에 final을 써야 하는 이유

final 키워드가 붙어있는데 생성자에 초기화 되지 않는다면 컴파일 에러가 발생하기 때문에 개발자의 실수를 사전에 막을 수 있다는 장점이 있기 때문입니다. 
이렇게 이번 글에서는 의존성 주입이란 무엇일까?에 대해서 간단하게 알아보았습니다. 지금은 마지막에 조금..? 스프링으로 의존성 주입하는 것을 해보았지만.. 처음에 보았던 예시는 스프링을 사용하지 않고 의존성 주입을 했던 예제입니다.

### Spring으로 의존성 주입하는 방법

위의 예처럼, 의존성 주입을 직접할 수도 있지만 스프링에서는 Bean으로만 등록되면 IoC 컨테이너가 의존성 주입을 알아서 해줍니다. Bean..?, IoC 컨테이너..? 생소한 용어가 나왔습니다. 하나씩 어떤 것인지 본격적으로 스프링에 대해서 알아보겠습니다.

### 의존성 주입, 어떤 방법으로 해야 할까?

대부분의 의존관계 주입은 한번 일어나면, 애플리케이션 종료시점까지 의존관계를 변경할 일이 없습니다. (오히려 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안됩니다. 불변해야 합니다.)
또 수정자 주입을 사용하면 setter 메소드를 public으로 열어두어야 합니다. 변경하면 안되는 메소드를 public으로 열어두면, 누군가 실수로 변경할 수도 있어 이는 좋은 설계가 아닙니다. 
생성자 주입은 객체를 생성할 때 딱 한번만 호출되므로 이후에 호출되는 일이 없습니다. 따라서 불변하게 설계할 수 있습니다.

# 결론

setter를 통한 의존성 주입 방식은 생성자 의존성 주입 방식에 비해, 잘 쓰이지 않습니다. 그 까닭은 생성자 생성을 잊거나, 변경되지 말아야 할 것이 변경될 수 있기 때문입니다. 

# 참고한 사이트

[https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/2%EC%A3%BC%EC%B0%A8/1.%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%EC%9D%B4%EB%9E%80%3F.md](https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/2%EC%A3%BC%EC%B0%A8/1.%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%EC%9D%B4%EB%9E%80%3F.md)