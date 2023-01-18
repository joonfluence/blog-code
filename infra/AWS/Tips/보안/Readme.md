### pem 키 분실 시 대처 방안

서두부터 결론적으로 말하자면 해당 키페어를 분실하면, 그 키페어를 다시 복구하는 방법은 없다.
따라서 키페어를 분실하여 ssh 접속을 못하는 인스턴스에 등록된 키페어를 다른 것으로 교체해서 다시 접속하게 해주는 방법으로 복구해야 한다.

### EC2 공개키 변경 방법

인스턴스 내부에서 authorized_keys의 내용을 변경하여 새로운 키 페어를 적용할 수 있다.

- Linux 인스턴스에 있는 ~/.ssh/authorized_keys 파일의 내용을 수정하여 새 공개 키로 교체하여 적용 하면 새로운 키로의 접근이 가능하며, 해당 방법의 경우 AWS Consol상에서 Key Name이 변경되지 않을 수 있음.

# 참고한 사이트

[pem 키 분실 시 방법](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-%ED%82%A4%ED%8E%98%EC%96%B4SSH-Key-%EB%B6%84%EC%8B%A4%EC%8B%9C-%EB%B3%B5%EA%B5%AC%ED%95%98%EB%8A%94-2%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
[https://velog.io/@jiyeon_hong/AWS-AWS-EC2-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%ED%82%A4-%ED%8E%98%EC%96%B4-%EA%B5%90%EC%B2%B4-%EB%B0%A9%EB%B2%95](https://velog.io/@jiyeon_hong/AWS-AWS-EC2-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%ED%82%A4-%ED%8E%98%EC%96%B4-%EA%B5%90%EC%B2%B4-%EB%B0%A9%EB%B2%95)
[AWS 비용절감 방법 10가지](https://aws.amazon.com/ko/blogs/korea/10-things-you-can-do-today-to-reduce-aws-costs/)
