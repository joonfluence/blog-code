### 서론

오늘은 프론트서버의 CI/CD 구축 방법에 대해서 알아보도록 하겠습니다.
Next.js 혹은 React.js의 빌드된 결과물을 상용서버에 바로 반영하는 방법 중 가능하도록 돕는 AWS의 Iaas인 Amplify에 대해서 알아보겠습니다.

### Amplify란

제공 기능은 다음과 같습니다. 그 중에서도 웹 앱 호스팅에 Amplify를 활용하는 방법에 대해서 알아보겠습니다.

- 앱 백엔드 생성
- 프론트 UI 구축
- **웹 앱 호스팅**

AWS Amplify는 사용 사례가 발전함에 따라 프런트엔드 웹 및 모바일 개발자가 다양한 AWS 서비스를 활용하는 유연성을 바탕으로 AWS에 풀 스택 애플리케이션을 손쉽게 구축, 배송 및 호스팅할 수 있도록 지원하는 완전한 솔루션입니다. 클라우드 전문 지식이 필요하지 않습니다.

### 조작 방법

- Amplify Studio(시각적 인터페이스)
- Amplify CLI(명령줄 인터페이스)
- Amplify 라이브러리(오픈 소스 클라이언트 라이브러리)
- Amplify 웹 호스팅(관리형 CI/CD 및 호스팅)
- Amplify UI 구성 요소(오픈 소스 설계 시스템)

### CI/CD 연동방법

- 서비스 선택
  - 서버에 이미 빌드되어있는 React 앱을 배포할 것이기 때문에 Deliver 항목의 Get started를 눌러 시작합니다.
    - ![image](./AWS%20Amplify%20로%20React%20앱%20CICD하기%202023-02-02%2008-28-18.png)
- Github 연동
  - 간편하게 Github에 있는 Repository를 이용하여 진행하였습니다. 권한을 부여해주고 CI/CD 연결을 위한 브랜치를 지정해줍니다.
    - ![image2](./AWS%20Amplify%20로%20React%20앱%20CICD하기%202023-02-02%2008-26-58.png)
    - ![image3](./AWS%20Amplify%20로%20React%20앱%20CICD하기%202023-02-02%2008-26-58.png)
  - 정상적으로 연동이 되었고, AWS 자체에서 도메인도 자동 생성해주었습니다.
    - ![image4](AWS%20Amplify%20로%20React%20앱%20CICD하기%202023-02-02%2008-27-08.png)
- 도메인 연결
  - 도메인을 Amazon Route53과 미리 연결해주면, 도메인 찾기 버튼을 누르니 자동 연결됩니다.
  - 도메인을 연결해주면 자동으로 무료 HTTPS 인증서도 발행해주고 있습니다.
  - DNS 공급자를 제공해주는 CDN 주소로 변경하여야 합니다.

### 비용

- 무료 조건
  - 월별 최대 1000 빌드 분 무료
  - 월별 최대 5GB 무료
  - 데이터 송신 월 최대 15GB 무료
  - 월별 최대 시간당 100GB 요청 수 무료
- 비용 조건
  - 호스팅 요금
    - 월별 제공되는 GB에 따라 0.15USD 씩 추가됨
    - 즉, 이용자가 많아질수록 호스팅 요금 증가.
      - ![iamge7](./AWS%20Amplify%20요금%20|%20프런트%20엔드%20웹%20및%20모바일%20|%20Amazon%20Web%20Services%202023-02-02%2008-33-28.png)
  - 빌드 요금
    - 웹 앱 크기가 클수록, 빌드를 여러번할수록 비용 증가.
      - ![image6](./AWS%20Amplify%20요금%20|%20프런트%20엔드%20웹%20및%20모바일%20|%20Amazon%20Web%20Services%202023-02-02%2008-33-28.png)

### 참고한 사이트

[https://aws.amazon.com/ko/amplify/](https://aws.amazon.com/ko/amplify/)
[https://aws.amazon.com/ko/amplify/pricing/](https://aws.amazon.com/ko/amplify/pricing/)
[https://velog.io/@primadonna/AWS-Amplify-%EB%A1%9C-React-%EC%95%B1-CICD%ED%95%98%EA%B8%B0](https://velog.io/@primadonna/AWS-Amplify-%EB%A1%9C-React-%EC%95%B1-CICD%ED%95%98%EA%B8%B0)
