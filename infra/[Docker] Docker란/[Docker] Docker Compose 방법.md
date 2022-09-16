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

Github Actions 통한 CI/CD 입니다.

```yml
name: Java CI with Gradle

on:
  push:
    branches:
      - master
      - release
permissions:
  contents: read
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: "11"
          distribution: "adopt"
      ## 1 Docker Hub에 이미지 push
      - name: Docker build
        run: |
          docker login -u ${{ secrets.USERNAME }} -p ${{ secrets.PASSWORD }}
          docker build -t dalicious77/spring-food-table .
          docker tag spring-food-table dalicious77/spring-food-table
          docker push dalicious77/spring-food-table
      ## 2 이미지를 서버에서 run
      - name: Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ubuntu
          key: ${{ secrets.PRIVATE_KEY }}
          envs: GITHUB_SHA
          script: |
            docker pull dalicious77/spring-food-table:${GITHUB_SHA::7}
            docker tag dalicious77/spring-food-table:${GITHUB_SHA::7} spring-food-table
            docker stop server
            docker run -d --rm --name server -p 3000:8080 spring-food-table
```
