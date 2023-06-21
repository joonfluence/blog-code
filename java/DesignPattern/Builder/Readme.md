## 서론

롬복의 @Builder 어노테이션과 빌더패턴에 대해 알아보도록 하겠습니다.

# 본론

### 개요

빌더 패턴은 객체를 생성할 때 사용하는 디자인 패턴 중 하나로, 동일한 프로세스를 거쳐 다양한 구성의 인스턴스를 만드는 방법 중 하나입니다. 
객체를 생성하는 클래스와 표한하는 클래스를 분리하여, 동일한 절차에서도 서로 다른 표현을 생성하는 방법을 제공합니다. 

### 빌더 패턴을 사용하는 이유

> Builder 패턴은 "복잡한 객체의 구성과 표현을 분리하여 동일한 구성 프로세스가 여러 개의 서로 다른 표현을 생성할 수 있도록" 하는 것을 목표로 합니다. - Gang of Four design patterns

제가 느낀 장점은 한 마디로 객체를 생성할 때, 빠진 값이 없도록 생성하도록 돕는다는 것 입니다. 객체를 생성하는 디자인 패턴 중 가장 효과적이기 때문에, 사용합니다. 

- 장점 
	- 편리함 : 필수 인자만을 생성자에 전부 전달하여 객체를 만듭니다. 생성해야 하는 객체가 Optional한 속성을 많이 가질수록 더 좋습니다. 
    - 가독성 : 선택 인자는 가독성이 좋은 코드로 인자를 넘길 수 있습니다. 
    - 불변 객체 : setter가 없으므로 객체 일관성을 유지하여 불변 객체로 생성할 수 있습니다. 

### 구현 방법

```java
package org.example;

public interface PostBuilder {
    PostBuilder title(String title);
    PostBuilder content(String content);
    PostBuilder author(String author);
    Post build();
}
```

먼저, 인터페이스를 만들어보겠습니다.

```java
package org.example;

public class Post implements PostBuilder{
    private String title;
    private String content;
    private String author;

    public Post(){}

    static Post builder(){
        return new Post();
    }

    @Override
    public PostBuilder title(String title) {
        this.title = title;
        return this;
    }

    @Override
    public PostBuilder content(String content) {
        this.content = content;
        return this;
    }

    @Override
    public PostBuilder author(String author) {
        this.author = author;
        return this;
    }

    public Post build(){
        return this;
    }

    public String getTitle(){
        return this.title;
    }

    public String getContent(){
        return this.content;
    }

    public String getAuthor(){
        return this.author;
    }
}
```

빌더패턴은 위와 같이 구현할 수 있고, 아래와 같이 사용할 수 있습니다. 

```java
package org.example;

public class Main {
    public static void main(String[] args) {
        String title = "빌더패턴에 대해서 알아보자.";
        String content = "오늘은 빌더패턴에 대해서 알아보겠습니다.";
        String author = "이준호";
        Post post = Post.builder().title(title).content(content).author(author).build();
        System.out.println(post.getTitle()); // 빌더패턴에 대해서 알아보자.
        System.out.println(post.getAuthor()); // 오늘은 빌더패턴에 대해서 알아보겠습니다.
        System.out.println(post.getContent()); // joonfluence
    }
}
```

### 그 외 객체 생성 디자인 패턴

- 점층적 생성자 패턴
    - 단점
    	- 생성자의 인자에 넣는 순서를 잘못 넣을 경우, 에러가 날 가능성이 존재합니다. 
        - Optional한 인자가 있을 경우, null 값을 넣어줘야 합니다. 

```java
public PostBuilder(String title, String content, String author){
    this.title = title;
    this.content = content;
    this.author = author;
}
```

점층적 생성자 패턴를 활용하면, 아래와 같이 객체를 생성할 수 있습니다. 

```java
package org.example;

public class Main {
    public static void main(String[] args) {
        DefaultPostBuilder post = new DefaultPostBuilder(
        	"빌더패턴에 대해서 알아보자.", // title
        	"오늘은 빌더패턴에 대해서 알아보겠습니다.", // content
            "joonfluence" // author
        );
        System.out.println(post.getTitle()); // 빌더패턴에 대해서 알아보자.
        System.out.println(post.getAuthor()); // 오늘은 빌더패턴에 대해서 알아보겠습니다.
        System.out.println(post.getContent()); // joonfluence
    }
}
```

- 자바 빈 패턴 
	- 장점
    	- 점층적 생성자 패턴보다 가독성이 낫습니다.
        - 순서에 상관 없이, 에러 발생 가능성이 줄어듭니다. 
    - 단점
    	- 함수 호출이 인자만큼 이뤄지고, 객체 호출 한번에 생성할 수 없습니다.
		- 불변 객체를 생성할 수 없습니다. 

```java
public Post {
    public void setTitle(String title){
        this.title = title;
    }
    public void setContent(String content){
        this.content = content;
    }
    public void setAuthor(String author){
        this.author = author;
    }
}
```

위와 같이, 생성자를 선언하면 아래와 같이 사용할 수 있습니다.

```java
package org.example;

public class Main {
    public static void main(String[] args) {
        String title = "빌더패턴에 대해서 알아보자.";
        String content = "오늘은 빌더패턴에 대해서 알아보겠습니다.";
        String author = "이준호";
		Post postSetter = new Post();
        postSetter.setTitle(title);
        postSetter.setContent(content);
        postSetter.setAuthor(author);
        System.out.println(postSetter.getTitle());
        System.out.println(postSetter.getAuthor());
        System.out.println(postSetter.getContent());
    }
}
```

### 결론

이상으로 다양한 객체 생성 방법에 대해서 알아보았습니다.

# 참고한 사이트

[https://dev-youngjun.tistory.com/197#:~:text=%EB%B9%8C%EB%8D%94%20%ED%8C%A8%ED%84%B4%EC%9D%80%20%EB%B3%B5%EC%9E%A1%ED%95%9C%20%EA%B0%9D%EC%B2%B4,%ED%95%98%EB%8A%94%20%EB%B0%A9%EB%B2%95%EC%9D%84%20%EC%A0%9C%EA%B3%B5%ED%95%9C%EB%8B%A4.](https://dev-youngjun.tistory.com/197#:~:text=%EB%B9%8C%EB%8D%94%20%ED%8C%A8%ED%84%B4%EC%9D%80%20%EB%B3%B5%EC%9E%A1%ED%95%9C%20%EA%B0%9D%EC%B2%B4,%ED%95%98%EB%8A%94%20%EB%B0%A9%EB%B2%95%EC%9D%84%20%EC%A0%9C%EA%B3%B5%ED%95%9C%EB%8B%A4.)
[https://hangjastar.tistory.com/246](https://hangjastar.tistory.com/246)