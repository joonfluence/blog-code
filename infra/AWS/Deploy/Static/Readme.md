# 서론

오늘은 정적 웹 사이트를 AWS에 배포 및 GitHub Actions를 통해 CI/CD 해보겠습니다.

# 본론

- AWS IAM 계정 생성
  - 루트 계정을 사용하는 것보다, IAM 사용자를 따로 두어 사용자를 구분하는 편이 보안상 낫다.
  - 사용자 자격 증명 유형에 액세스 키, 암호를 모두 체크한다. 액세스 키는 GitHub Actions 환경 또는 설명에 적혀있는 것처럼 터미널과 같은 CLI(Command Line Interface) 환경에서 AWS 작업을 하기 위해 필요하다. 암호는 해당 사용자가 AWS 웹 페이지에 암호를 사용할 수 있게 해준다. 
  - 주의사항 : 액세스 키를 잃어버리면 기존 키를 삭제하고 재생성해야 한다.
- 권한 부여 : 더 제한된 권한을 부여할 수도 있지만, 원활한 개발을 위해선 위 서비스에 대한 Full Access 지정해주는 편이 좋다.
  - S3
    - AmazonS3FullAccess
  - Route53
    - AmazonRoute53FullAccess
  - CloudFront : CDN 캐쉬 서버
    - AmazonCloudFrontFullAccess
  - ChangePassword
    - IAMUserChangePassword
  - ACM(Aws Certificate Manager) : AWS의 HTTPS 발급 서비스
    - AWSCertificateManagerFullAccess
- IAM 계정 로그인
- Route 53을 통해 다른 AWS 서비스와의 연동을 위한 도미엔 관리 기능을 사용한다. 
  - 호스팅 생성
  - 네임서버 등록
- ACM를 통해 HTTPS를 위한 SSL/TLS 인증서를 발급 받는다.
  - 인증서 생성 
- AWS S3를 통해 정적 웹 사이트를 호스팅한다. 
  - 버킷 생성
  - 업로드 
    - 직접 업로드하거나 cli를 통해 업로드 할 수 있다. 
    ```shell
    aws s3 
    ```
  - 정적 웹 사이트 배포
    - 정적 웹 사이트 호스팅 
      - 인덱스 문서와 오류 문서 모두 index.html로 설정 
  - 버킷 권한 수정
    - 403 Forbidden 오류 발생되므로 버킷의 권한을 `모든 퍼블릭 액세스 차단 비활성화` 
  - 정책 생성기 
    - 정책 유형 선택
    - 정책 유형 허용 
    - 특정 부분에만 권한 부여 
    - 어떤 권한을 부여할 것인지 선택 
- CloudFront 서비스를 통해 S3로 호스팅된 정적 웹 사이트와 구매한 도메인을 연결하고 HTTP 접근을 HTTPS로 리다리렉션 해준다. 
  - 배포 생성 
  - 뷰어 프로토콜 정책 : Redirect HTTP to HTTPS
  - 대체 도메인 이름, 사용자 정의 SSL 인증서, 기본값 루트 객체 입력
- 배포된 CloudFront 주소와 Route53 서비스를 연결한다. 
  - 이미 배포된 주소가 있으므로 레코드 유형을 A로 설정한다. A 레코드란 IPv4 주소 및 일부 AWS 리소스로 트래픽을 라우팅하는 것을 말함. 
- GitHub Actions를 통한 CI/CD
  - 스크립트 작성
  ```yaml
  name: Next.js deployment practice

  on:
    push:
      branches: [main]

  jobs:
    continuous-deployment:
      runs-on: ubuntu-latest
      steps:
        - name: Git Checkout
          uses: actions/checkout@v2

        - name: Use Node.js version 14.x
          uses: actions/setup-node@v1
          with:
            node-version: 14.x

        - name: Build
          run: |
            npm install -g yarn
            yarn install --frozen-lockfile
            yarn build

        - name: Configure AWS credentials
          uses: aws-actions/configure-aws-credentials@v1
          with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            aws-region: ${{ secrets.AWS_REGION }}

        - name: Deploy to S3
          run: aws s3 sync ./${{ secrets.BUILD_DIRECTORY }} ${{ secrets.AWS_S3_BUCKET_NAME }} --acl public-read --delete

        - name: CloudFront Invalidate Cache
          run: aws cloudfront create-invalidation --distribution-id ${{ secrets.AWS_CLOUDFRONT_DISTRIBUTION_ID }} --paths '/*'
    ```
  - GitHub Secret 작성

# 참고한 사이트

(https://weekwith.tistory.com/entry/Nextjs-AWS-S3%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%A0%95%EC%A0%81-%EC%9B%B9-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EB%B0%B0%ED%8F%AC-%EB%B0%8F-GitHub-Actions%E1%84%85%E1%85%B3%E1%86%AF-%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB-CICD)[https://weekwith.tistory.com/entry/Nextjs-AWS-S3%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%A0%95%EC%A0%81-%EC%9B%B9-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EB%B0%B0%ED%8F%AC-%EB%B0%8F-GitHub-Actions%E1%84%85%E1%85%B3%E1%86%AF-%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB-CICD]
