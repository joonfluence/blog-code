### Conventions

1. Id는 BigInt, UN/AI/NN/PK 설정 무조건 해주세요.
2. 테이블 명명 규칙
  - 모듈__이름(user__role, user__user)
  - 다 소문자로 해주세요 
  - 단축어 쓰지 말죠(id처럼 공용적인거 말고는 - pwd말고 그냥 password쓰기)
3. createdAt은TIMESTAMP 쓰기
4. 크기가 딱 한정될 때는 VARCHAR 11 등으로 딱 맞춰준다(휴대폰번호)
5. 이메일주소 맥시멈 값은 254(통신규약). 
6. 그렇지 않은경우 2의 N승으로 간다(예외 - 256은 255로 씀(이진수로 표기할때 앞에 1자리는 +.-표기를 위해서 밀림))
7. 패스워드 같은것은 BINARY로 설정
8. BOOL값은 BIT로. 
9. e_를 앞에 붙이는거는 LOOKUP테이블을 정의할때
    - Enum으로 정의한다라는 의미
10. 모든 필드에 Comment 추가해주세요.
11. PKey -> id
12. 위도, 경도는 데이터타입 POINT로 작성