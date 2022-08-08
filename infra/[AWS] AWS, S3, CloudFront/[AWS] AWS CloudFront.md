# 서론



# 본론



### 관련 키워드

- Content Delivery Network(CDN) : 실제론 데이터센터. 
- Origin Server : 일반적으로 원본 데이터를 보관하고 있는 스토리지, S3 등을 말함.
- Edge Server = Edge Location : AWS에서 실질적으로 제공하는 전 세계에 퍼져있는 서버를 말함. 

### 실제 적용해보기 

1. CloudFront > Create Distribution 
2. 분배할 컨텐츠의 방법 선택 (Web) 
3. Origin Domain Name ex) S3 스토리지 
4. Default Cache Settings 
5. Distribution Settings 
  - Logging on 
  - Cookie Logging on 
6. S3 > 객체소유권 > ACL 활성화 > ACL 복원 허용 : 로깅 설정 
7. cloudFront > OAI(Origin Access Identity) > use an Existing Identity 

# 마무리 

# 참고한 사이트

[https://dev.classmethod.jp/articles/how-fast-is-cloudfront-speed-test/](https://dev.classmethod.jp/articles/how-fast-is-cloudfront-speed-test/)
[https://real-dongsoo7.tistory.com/86](https://real-dongsoo7.tistory.com/86)
[https://dev.classmethod.jp/articles/the-s3-bucket-that-you-specific-for-cloudfront-logs-does-not-enable-acl-access-error-resolution-during-log-setup/](https://dev.classmethod.jp/articles/the-s3-bucket-that-you-specific-for-cloudfront-logs-does-not-enable-acl-access-error-resolution-during-log-setup/)
[https://lemontia.tistory.com/1032](https://lemontia.tistory.com/1032)