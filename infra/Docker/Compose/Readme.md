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