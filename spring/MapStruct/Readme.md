# 본론

### 정의

MapStruct란 Entity와 Dto간의 매핑을 지원하는 라이브러리를 말한다.

### Entity to DTO

데이터 조회 API를 개발하는 경우를 생각해보자. 컨트롤러를 통해서 들어온 요청이 여러 비즈니스 로직과 데이터 접근 로직을 거치면서 여러 계층 간의 데이터 변경을 한 후에 최종적으로 응답을 반환한다.
DTO(Data Transfer Object)와 Entity 간의 변환을 예로 들 수 있다. 보통 빌더를 사용했다면 아래와 같은 형태로 객체 간의 매핑을 했얼 것이다.

```java
ArticleEntity toEntity(Article article) {
  return ArticleEntity.builder()
    .id(article.getId())
    .articleTypeCode(article.getType().getCode())
    .title(article.getTitle())
    .author(article.getWriter())
    .build();
}
```

MapStruct 라이브러리를 사용하면 어떻게 코드를 편리하게 변경할 수 있을까? 지금부터 사용 방법을 알아보자.

### 사용하는 이유

Entity To DTO 작업을 쉽게 만들어준다.

### 환경 설정

MapStruct 라이브러리를 사용하려면 의존성 추가가 필요하다. build.gradle 파일에 아래와 같이 선언하자. 아래 목록에 보이는 `lombok-mapstruct-binding` 의존성은 Lombok 1.18.16 버전부터 추가되었는데, Lombok과 MapStruct 라이브러리를 같이 사용할 때 발생하는 순서 충돌 문제를 해결해준다.

```groovy
// build.gradle
dependencies {
    // lombok
    implementation 'org.projectlombok:lombok:1.18.22'
    annotationProcessor 'org.projectlombok:lombok:1.18.22'
    annotationProcessor 'org.projectlombok:lombok-mapstruct-binding:0.2.0' // v1.18.16+ 부터

    // mapstruct
    implementation 'org.mapstruct:mapstruct:1.4.2.Final'
    annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'
}
```

### 적용

MapStruct를 사용하면, @Mapper 어노테이션을 통해 손쉽게 

```java
// GenericMapper interface
public interface GenericMapper<D, E> {

    D toDto(E e);
    E toEntity(D d);

    @BeanMapping(nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE)
    void updateFromDto(D dto, @MappingTarget E entity);
}
```

```java
@Mappper(componentModel = "spring")
public interface CodingRoomMapper extends GenericMapper<CodingRoomDto, CodingRoom>{

}
```

- @Mapper : MapStruct Code Generator가 해당 인터페이스의 구현체를 생성해준다.
- componentModel = "spring" : spring에 맞게 bean으로 등록해준다

```java
// CodingRoomMapperImpl class
@Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2021-05-28T02:50:51+0900",
    comments = "version: 1.4.1.Final, compiler: javac, environment: Java 1.8.0_231 (Oracle Corporation)"
)
@Component
public class CodingRoomMapperImpl implements CodingRoomMapper {

    @Override
    public CodingRoomDto toDto(CodingRoom arg0) {
        if ( arg0 == null ) {
            return null;
        }

        CodingRoomDto codingRoomDto = new CodingRoomDto();

        if ( arg0.getKey() != null ) {
            codingRoomDto.setKey( String.valueOf( arg0.getKey() ) );
        }
        codingRoomDto.setTitle( arg0.getTitle() );
        codingRoomDto.setIntro( arg0.getIntro() );
        codingRoomDto.setPassword( arg0.getPassword() );
        codingRoomDto.setMaxUser( arg0.getMaxUser() );
        codingRoomDto.setRamdomKey( arg0.getRamdomKey() );
        codingRoomDto.setCreatedAt( arg0.getCreatedAt() );
        codingRoomDto.setCodingJoinUsers( codingJoinUserListToCodingJoinUserDtoList( arg0.getCodingJoinUsers() ) );
        codingRoomDto.setCodingTests( codingTestListToCodingTestDtoList( arg0.getCodingTests() ) );

        return codingRoomDto;
    }

    @Override
    public CodingRoom toEntity(CodingRoomDto arg0) {
        if ( arg0 == null ) {
            return null;
        }

        CodingRoom codingRoom = new CodingRoom();

        if ( arg0.getKey() != null ) {
            codingRoom.setKey( Long.parseLong( arg0.getKey() ) );
        }
        codingRoom.setTitle( arg0.getTitle() );
        codingRoom.setIntro( arg0.getIntro() );
        codingRoom.setPassword( arg0.getPassword() );
        codingRoom.setMaxUser( arg0.getMaxUser() );
        codingRoom.setRamdomKey( arg0.getRamdomKey() );
        codingRoom.setCreatedAt( arg0.getCreatedAt() );
        codingRoom.setCodingJoinUsers( codingJoinUserDtoListToCodingJoinUserList( arg0.getCodingJoinUsers() ) );
        codingRoom.setCodingTests( codingTestDtoListToCodingTestList( arg0.getCodingTests() ) );

        return codingRoom;
    }
...
}
```

```java
@Service
@RequiredArgsConstructor
public class CodingRoomService {
  
  private final CodingRoomMapper codingRoomMapper;
  ...
      
  CodingRoomDto codingRoomDto =  codingRoomMapper.toDto(codingRoom);
  ...        
}
```

각 구현체를 받아와서 사용하면 된다. 

# 참고한 사이트

[Mapstruct-in-springboot](https://madplay.github.io/post/mapstruct-in-springboot)
