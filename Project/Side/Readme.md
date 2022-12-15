### 레퍼런스

- 무다-감정일기(앱)
  - 감정을 선택하기 위한 인터랙션을 여러가지 생각해보자.
  - '바텀바' 두고 선택 가능하도록 만들자.
  - 그중에서도 역시 감정을 드러내는 건 이모지다. 😀😁🥲😕🤩🥰😎😨.
  - 애니메이션 : 이모지를 선택하는 방식을 따라하자.
  - 화면 구성 : 그대로 따라하자.


**기술스택**

[배포환경]

- AWS EC2 Free-tier
- AWS RDS 구동
- AWS CodeDeploy + Docker

[로컬환경]

- Mac
- MariaDB

[개발스택]

- FE : React
- BE : Express + Node.js
  - framework : express
  - logging : morgan
  - process manager : PM2
- Infra : AWS + Docker
