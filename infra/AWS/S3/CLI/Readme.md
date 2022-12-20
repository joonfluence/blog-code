### CLI 통해 S3 배포하는 방법

- AWS access key 발급 받기

액세스 키는 자격 증명을 판단하는 수단임. AWS CLI 또는 AWS API에 대한 프로그래밍 요청에 서명할 수 있음.

```shell

```

- 설치 라이브러리

```shell
pip install awscli
```

- 설치 확인 및 정보 설정

```shell
aws help # aws 설치 확인
aws configure
```

- GitHub Actions Script

```yml
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

# 참고한 사이트

[https://lovit.github.io/aws/2019/01/30/aws_s3_iam_awscli/](https://lovit.github.io/aws/2019/01/30/aws_s3_iam_awscli/)
