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

반면 Car가 직접 Tire를 주입하는 것이 아니라 외부에서 주입을 받는다면 Car 클래스에는 수정이 일어나지 않아도 되고 따로 모듈화하여 뺄 수도 있습니다.

### 필드에 final을 써야 하는 이유

final 키워드가 붙어있는데 생성자에 초기화 되지 않는다면 컴파일 에러가 발생하기 때문에 개발자의 실수를 사전에 막을 수 있다는 장점이 있기 때문입니다. 
이렇게 이번 글에서는 의존성 주입이란 무엇일까?에 대해서 간단하게 알아보았습니다. 지금은 마지막에 조금..? 스프링으로 의존성 주입하는 것을 해보았지만.. 처음에 보았던 예시는 스프링을 사용하지 않고 의존성 주입을 했던 예제입니다.

### Spring으로 의존성 주입하는 방법

위의 예처럼, 의존성 주입을 직접할 수도 있지만 스프링에서는 Bean으로만 등록되면 IoC 컨테이너가 의존성 주입을 알아서 해줍니다. Bean..?, IoC 컨테이너..? 생소한 용어가 나왔습니다. 하나씩 어떤 것인지 본격적으로 스프링에 대해서 알아보겠습니다.

# 참고한 사이트

[https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/2%EC%A3%BC%EC%B0%A8/1.%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%EC%9D%B4%EB%9E%80%3F.md](https://github.com/wjdrbs96/Gyunny_Spring_Study/blob/master/spring/2%EC%A3%BC%EC%B0%A8/1.%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%EC%9D%B4%EB%9E%80%3F.md)