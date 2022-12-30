### Linux Zip 파일 압축 해체하는 방법

- gz 압축 하기

```shell
gzip {압축 파일명}
```

- 파일 압축하기

aaa.jpg를 gz으로 압축한다면 아래와 같은 명령어를 사용하면 된다.

```
gzip aaa.jpg
```

수행 결과로 aaa.jpg는 없어지고, aaa.gz 압축 파일이 생성된다.

gz은 여러개의 파일을 하나로 압축하는 용도가 아니다.
여러개의 파일을 압축하시려면 tar, zip, 7z 압축을 사용하시길 바란다.

- gz 압축 풀기

```shell
gzip -d {압축 파일명}.gz
```

gzip으로 압축을 푸는 옵션으로 -d 를 주면 된다. `-d`는 `decompress의 줄임 표현`이다.

- gzip 설치하기

gzip은 리눅스에 기본으로 설치되어 있으나, 혹시 gzip 명령어를 찾을 수 없다고 나오면 아래 명령어로 설치하면 된다.

우분투에서 unzip 설치

```shell
apt-get install gzip
```

[https://araikuma.tistory.com/121](https://araikuma.tistory.com/121)
