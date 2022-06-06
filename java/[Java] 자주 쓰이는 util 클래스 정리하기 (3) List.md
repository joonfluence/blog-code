# 서론

오늘은 자바 프로그래밍을 하면서 유용하게 쓰이는 util 클래스 중 하나인 List에 관해 정리하는 시간을 갖겠습니다.

# 본론

### 컬렉션 프레임워크와의 관계

List 인터페이스는 컬렉션 프레임워크를 상속 받습니다. 따라서 컬렉션 메서드를 그대로 사용할 수 있죠. 단, 내부적으로 구현체에 따라 구현 방식과 특성에는 차이가 있습니다.

### List의 구현체 종류

또 List는 인터페이스이기 때문에, 이를 구현한 구현체는 여러 개일 수 있습니다. 그러나 동일한 메서드를 갖죠. 아래와 같이 말이죠.

- ArrayList
- LinkedList
- Vector
- Stack

### List의 특성

List 인터페이스는 중복을 허용하면서 저장순서를 유지하는 특성을 갖습니다.

### List 구현체 메서드의 종류

1. 생성

- add() : adds an element to a list
- addAll() : adds all elements of one list to another

```java

```

2. 조회/순회

- get() : helps to randomly access elements from lists
- iterator() : returns iterator object that can be used to sequentially access elements of lists
- set() : changes elements of lists

3. 삭제

- remove() : removes an element from the list.
- removeAll() : removes all the elements from the list.
- clear() : removes all the elements from the list (more efficient than removeAll()).

4. 기타

- size() : returns the length of lists.
- toArray() : converts a list into an array.
- contains() : returns true if a list contains specified element.

### ArrayList

먼저 ArrayList는 Array와 어떤 차이점이 있을까요?

Array와의 차이점은 Array는 length 제한이 있지만, ArrayList에는 갯수의 제한이 없습니다.
또한 ArrayList의 내부 구현체를 살펴보면, ArrayList 내부적으로는 Array를 이미 사용하고 있다는 걸 알 수 있습니다.

## LinkedList

### 정의

### 선언방법

### 생성

● add : 데이터를 추가한다. (인덱스 지정 가능)

### 조회

● get : 특정 인덱스의 데이터를 반환한다.

### 삭제

● remove : 데이터를 삭제한다. (인덱스 혹은 데이터 지정가능)

### 기타

● size: 아이템 갯수를 반환
● contains: 특정데이터가 존재하는지 여부 반환

### 수정

- [ArrayList](https://wikidocs.net/207#arraylist)
  - [add](https://wikidocs.net/207#add)
  - [get](https://wikidocs.net/207#get)
  - [size](https://wikidocs.net/207#size)
  - [contains](https://wikidocs.net/207#contains)
  - [remove](https://wikidocs.net/207#remove)

[https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html)
[List (Java SE 11 & JDK 11)]
An ordered collection (also known as a sequence). The user of this interface has precise control over where in the list each element is inserted. The user can access elements by their integer index (position in the list), and search for elements in the list.
[docs.oracle.com](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html)
