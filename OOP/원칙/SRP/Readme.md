# 본론

오늘은 객체지향설계 5원칙 중 하나인 SRP에 관해서 알아보도록 하겠습니다.

## 단일 책임의 원칙(SRP, Single Responsibility Principle)

> 클래스를 변경하는 이유는 단 한 가지여야 한다. - Robert. C. Martin

하나의 모듈은 한 가지 책임을 가져야 한다는 것으로, 이것은 모듈이 변경되는 이유가 한가지여야 함을 의미합니다. 여기서 변경의 이유가 한가지라는 것은 해당 모듈이 여러 대상 또는 액터들에 대해 책임을 가져서는 안되고, **오직 하나의 액터에 대해서만 책임을 져야 한다는 것**을 의미합니다.

![SRP](./SRP.png)
--> 한줄 다이어그램 설명

### 적용사례

예를 들어 다음과 같이 입력으로 사용자의 정보를 받아서, 비밀번호를 암호화하여 데이터베이스에 저장하는 로직이 있다고 하자.

단일 책임 원칙을 제대로 지키면 변경이 필요할 때 수정할 대상이 명확해진다. 그리고 이러한 단일 책임 원칙의 장점은 시스템이 커질수록 극대화되는데, 시스템이 커지면서 서로 많은 의존성을 갖게되는 상황에서 변경 요청이 오면 딱 1가지만 수정하면 되기 때문이다.

단일 책임 원칙을 적용하여 적절하게 책임과 관심이 다른 코드를 분리하고, 서로 영향을 주지 않도록 추상화함으로써 애플리케이션의 변화에 손쉽게 대응할 수 있다.

```java
@Service
@RequiredArgsConstructor
public class UserService {

	private final UserRepository userRepository;

	public void addUser(final String email, final String pw) {
		final StringBuilder sb = new StringBuilder();

		for(byte b : pw.getBytes(StandardCharsets.UTF_8)) {
			sb.append(Integer.toString((b & 0xff) + 0x100, 16).substring(1));
		}

		final String encryptedPassword = sb.toString();
		final User user = User.builder()
				.email(email)
				.pw(encryptedPassword).build();

		userRepository.save(user);
	}
}
```

### 적용 이전

```java
@Component
public class SimplePasswordEncoder {

	public void encryptPassword(final String pw) {
		final StringBuilder sb = new StringBuilder();

		for(byte b : pw.getBytes(StandardCharsets.UTF_8)) {
			sb.append(Integer.toString((b & 0xff) + 0x100, 16).substring(1));
		}

		return sb.toString();
	}
}

@Service
@RequiredArgsConstructor
public class UserService {

	private final UserRepository userRepository;
	private final SimplePasswordEncoder passwordEncoder;

	public void addUser(final String email, final String pw) {
		final String encryptedPassword = passwordEncoder.encryptPassword(pw);

		final User user = User.builder()
				.email(email)
				.pw(encryptedPassword).build();

		userRepository.save(user);
	}
}
```

### 적용 이후

단일 책임 원칙을 제대로 지키면 변경이 필요할 때 수정할 대상이 명확해집니다. 그리고 이러한 단일 책임 원칙의 장점은 시스템이 커질수록 극대화되는데, 시스템이 커지면서 서로 많은 의존성을 갖게되는 상황에서 변경 요청이 오면 딱 1가지만 수정하면 되기 때문입니다.

단일 책임 원칙을 적용하여 적절하게 책임과 관심이 다른 코드를 분리하고, 서로 영향을 주지 않도록 추상화함으로써 애플리케이션의 변화에 손쉽게 대응할 수 있습니다.

### SRP 규칙을 지키고 있는지 확인하는 방법

OOO 클래스에 대한 SRP 분석

- OOO가(이) 자신을 OOO 한다.
- OOO가(이) 자신을 OOO 한다.
- OOO가(이) 자신을 OOO 한다.

제목에 내가 만들고 싶은 클래스 이름을 적고, 그 아래 내가 계획한 메서드들을 적어본다.

ex) 휴대폰 클래스에 대한 SRP 분석

- 휴대폰가(이) 자신을 음성송신한다. (O)
- 휴대폰가(이) 자신을 음성수신한다. (O)
- 휴대폰가(이) 자신을 기기충전한다. (X)
  - 사람이 휴대폰을 기기충전한다. (O)
- 휴대폰가(이) 자신을 전화걸기한다. (X)
  - 사람이 휴대폰을 전화걸기한다. (O)
- 휴대폰가(이) 자신을 전화받기한다. (X)
  - 사람이 휴대폰을 전화받기한다. (O)

# 결론

짧게 SRP에 관해서 알아보았습니다. 읽어주셔서 감사합니다.

# 참고한 사이트

[https://mangkyu.tistory.com/194](https://mangkyu.tistory.com/194)
