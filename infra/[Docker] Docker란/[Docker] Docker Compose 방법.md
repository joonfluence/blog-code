```yml
version: "3"
services:
  web: # nginx 컨테이너 이름 (원하는 이름)
    image: nginx
    ports:
      - 8080:80
    volumes:
      - /etc/nginx/:/etc/nginx/ # Nginx 컨테이너 내부 /etc/nginx 디렉토리가 위에서 설치한 EC2 내부에 /etc/nginx 디렉토리를 참조함
  spring: # Spring Boot 컨테이너 이름 (원하는 이름)
    build: .
    ports:
      - 8080:8080
    volumes:
      - ./:/root/
```

### Github Actions 통한 CI/CD

```yml
name: Java CI with Gradle

on:
  push:
    branches:
      - main
      - release
permissions:
  contents: read
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: "11"
          distribution: "temurin"

      ## 1-1 application.properties 파일 생성
      - name: make application.properties
        run: |
          cd ./src/main/resources
          touch ./application.properties
          echo "${{ secrets.PROPERTIES }}" > ./application.properties
        shell: bash

      ## 1-2 application-aws.yml 파일 생성
      - name: make application-aws.yml
        run: |
          cd ./src/main/resources
          touch ./application-aws.yml
          echo "${{ secrets.AWS_PROPERTIES }}" > ./application-aws.yml
        shell: bash

      ## 1-3 application-oauth.properties 파일 생성
      - name: application-oauth.properties
        run: |
          cd ./src/main/resources
          touch ./application-oauth.properties
          echo "${{ secrets.OAUTH_PROPERTIES }}" > ./application-oauth.properties
        shell: bash

      ## 2 스프링 프로젝트 jar 파일 빌드
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
      - name: Build with Gradle
        run: ./gradlew build

      ## 3 Docker Hub에 이미지 push
      - name: Docker build
        run: |
          docker login -u ${{ secrets.USERNAME }} -p ${{ secrets.PASSWORD }}
          docker build -t test .
          docker tag test qwe382/tikkeeul:${GITHUB_SHA::7}
          docker push qwe382/tikkeeul:${GITHUB_SHA::7}

      ## 4 이미지를 서버에서 run
      - name: Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ubuntu
          key: ${{ secrets.PRIVATE_KEY }}
          envs: GITHUB_SHA
          script: |
            docker pull qwe382/tikkeeul:${GITHUB_SHA::7}
            docker tag qwe382/tikkeeul:${GITHUB_SHA::7} tikkeeul
            docker stop server
            sleep 5
            docker run -d --rm --name server -p 80:8080 tikkeeul
```
