# 본론

### 정의

대체키(Alternate key)는 후보키가 2개 이상일때, 그 중에서 어느 하나를 기본키로 지정하고 남은 후보키를 대체키라고 한다. 예시는 아래와 같다.

![alternative_key.png](./%08alternative_key.png)

### 보안 측면에서의 대체키의 필요성

대체키란 용어는 자연키라는 용어와 대칭되는 개념인데, 이 강의에서는 Entity의 식별자와 동급의 의미를 가지는 추가 식별자 정도로 용어를 정의하고자 한다.
Entity의 식별자는 외부에 오픈하거나 오용되지 않도록 주의하고, 식별자가 아닌 대체키를 오픈하는 것이 여러모로 좋다.

### 대체키의 구현 방법

시스템 내부에서의 Entity 식별자는 Long 타입의 id를 사용하고, 외부에 오픈하여 사용할 때는 대체키를 사용한다. 대체키는 String 기반의 token을 생성하고 unique index로 설정하여 사용한다.

# 참고한 사이트

[http://wiki.hash.kr/index.php/%EB%8C%80%EC%B2%B4%ED%82%A4](http://wiki.hash.kr/index.php/%EB%8C%80%EC%B2%B4%ED%82%A4)
