# 본론

### S3란

Simple Storage Service을 줄여서, S3로 지칭합니다. 이름에서 알 수 있듯, S3는 `파일 관리 시스템`입니다.

### S3의 이점

1. 99.999999%의 내구성을 지닙니다.

- AWS S3의 장점은 파일이 하나의 서버에만 저장하는 게 아니라, 여러 지역에 위치한 각기 독립적인 컴퓨터에 해당 정보를 보관한다는 점입니다. 즉, 하나의 서버에서 문제가 발생하더라도 해당 파일이 사라지는 것은 아닙니다.

2. 파일 보관 뿐 아니라, 공유도 쉽다.

- 언제든지 해당 파일을 다운로드할 수 있도록 접근 링크를 제공합니다.

3. 파일 서버로도 사용할 수 있다.

- 프론트엔드 서버를 S3에 올려, 유저 트래픽을 받을 수도 있습니다.

4. 버젼관리 기능을 제공합니다.

- 파일 하나를 계속 수정하더라도 업로드된 과거 파일을 추적할 수 있습니다.

### S3의 구성요소

1. 버킷

- 하나의 프로젝트를 말합니다.

2. 폴더

- 파일을 담는 공간을 말합니다.

3. 객체 (Object)

- S3에선 파일을 Object라고 지칭합니다.

### S3 사용방법

1. 로그인
2. S3로 이동 후, 버킷 만들기
3. 버킷 이름과 리전 선택
4. 퍼블릭 액세스 설정
5. 버킷 버전 관리 및 기본 암호화
6. 퍼블릭 정책 활성화
7. 테스트
8. 액세스 키 설정

### S3 사용 요금

S3는 사용자가 실제 사용한 만큼만 요금을 책정합니다.

### 실제 코드

1. 라이브러리 설치

build.gradle

```groovy
implementation 'com.amazonaws:aws-java-sdk-s3:1.12.276'
```
2. 코드 작성

```java
package yewoon.server.post.service;

import com.amazonaws.AmazonServiceException;
import com.amazonaws.SdkClientException;
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.s3.model.CopyObjectRequest;
import com.amazonaws.services.s3.model.DeleteObjectRequest;
import com.amazonaws.services.s3.model.ObjectMetadata;
import com.amazonaws.services.s3.model.PutObjectRequest;
import java.io.File;
import java.io.InputStream;

public class AwsS3 {
    //Amazon-s3-sdk
    private AmazonS3 s3Client;
    final private String accessKey = "액세스키";
    final private String secretKey = "시크릿키";
    private Regions clientRegion = Regions.AP_NORTHEAST_2;
    private String bucket = "yeowoonbucket";

    private AwsS3() {
        createS3Client();
    }

    //singleton pattern
    static private AwsS3 instance = null;

    public static AwsS3 getInstance() {
        if (instance == null) {
            return new AwsS3();
        } else {
            return instance;
        }
    }

    public AmazonS3 getS3Client() {
        return s3Client;
    }

    //aws S3 client 생성
    private void createS3Client() {

        AWSCredentials credentials = new BasicAWSCredentials(accessKey, secretKey);
        this.s3Client = AmazonS3ClientBuilder
                .standard()
                .withCredentials(new AWSStaticCredentialsProvider(credentials))
                .withRegion(clientRegion)
                .build();
    }

    public void upload(File file, String key) {
        uploadToS3(new PutObjectRequest(this.bucket, key, file));
    }

    public void upload(InputStream is, String key, String contentType, long contentLength) {
        ObjectMetadata objectMetadata = new ObjectMetadata();
        objectMetadata.setContentType(contentType);
        objectMetadata.setContentLength(contentLength);

        uploadToS3(new PutObjectRequest(this.bucket, key, is, objectMetadata));
    }

    //PutObjectRequest는 Aws S3 버킷에 업로드할 객체 메타 데이터와 파일 데이터로 이루어져있다.
    private void uploadToS3(PutObjectRequest putObjectRequest) {

        try {
            this.s3Client.putObject(putObjectRequest);
            System.out.println(String.format("[%s] upload complete", putObjectRequest.getKey()));

        } catch (AmazonServiceException e) {
            e.printStackTrace();
        } catch (SdkClientException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void copy(String orgKey, String copyKey) {
        try {
            //Copy 객체 생성
            CopyObjectRequest copyObjRequest = new CopyObjectRequest(
                    this.bucket,
                    orgKey,
                    this.bucket,
                    copyKey
            );
            //Copy
            this.s3Client.copyObject(copyObjRequest);

            System.out.println(String.format("Finish copying [%s] to [%s]", orgKey, copyKey));

        } catch (AmazonServiceException e) {
            e.printStackTrace();
        } catch (SdkClientException e) {
            e.printStackTrace();
        }
    }

    public void delete(String key) {
        try {
            //Delete 객체 생성
            DeleteObjectRequest deleteObjectRequest = new DeleteObjectRequest(this.bucket, key);
            //Delete
            this.s3Client.deleteObject(deleteObjectRequest);
            System.out.println(String.format("[%s] deletion complete", key));

        } catch (AmazonServiceException e) {
            e.printStackTrace();
        } catch (SdkClientException e) {
            e.printStackTrace();
        }
    }
}
```

# 결론

오늘은 간단하게 AWS S3에 관해서 알아보았습니다.

# 참고한 사이트

[https://bamdule.tistory.com/177](https://bamdule.tistory.com/177)
[https://bamdule.tistory.com/178](https://bamdule.tistory.com/178)