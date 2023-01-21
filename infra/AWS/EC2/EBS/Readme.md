### EBS란

- EC2의 디스크로 사용됨.
  - SSD, HDD 제공.
  - 스냅숏 제공.
  - EBS 스냅숏은 S3에 저장됨.
- 추가 비용 없이 EBS 암호화가 가능함.

### 디스크 용량 늘리는 법

1. ELB 용량 늘리기

2. 파티션 늘리기

```shell
sudo growpart /dev/xvda 1
```

3. ex4 파일 시스템 확장

```shell
sudo resize2fs /dev/xvda1 # ubuntu
sudo xfs_growfs /dev/xvda # centOs
```

# 참고한 사이트

[https://ithub.tistory.com/m/253](https://ithub.tistory.com/m/253)
[https://brunch.co.kr/@topasvga/2501](https://brunch.co.kr/@topasvga/2501)
[https://leonkim.dev/aws/ce2-increase-disk-space/](https://leonkim.dev/aws/ce2-increase-disk-space/)
[https://nirsa.tistory.com/231](https://nirsa.tistory.com/231)
