### 트랜잭션

- 서버에서의 트랜잭션
  - In the context of servers, a transaction refers to a series of operations that are executed as a single unit of work.
- DB에서의 트랜잭션

### AWS에서 TPS 지표 살펴보는 방법

- EC2

  - CloudWatch Metrics: You can use Amazon CloudWatch to monitor the performance metrics of your Amazon EC2 instances, including TPS. To do this, you'll need to configure your EC2 instances to send performance data to CloudWatch. You can then use the CloudWatch console or API to view the TPS metrics.
  - Application Load Balancer (ALB) Access Logs: If you are using an Application Load Balancer (ALB), you can enable access logs, which provide detailed information about each request processed by the load balancer. You can use the log data to calculate TPS by counting the number of requests per second.
  - Custom Metrics: If you want to monitor TPS for a specific application or service, you can instrument your code to emit TPS metrics to CloudWatch. You can use CloudWatch APIs or libraries provided by AWS to send custom metrics to CloudWatch.
  - Third-Party Monitoring Tools: There are several third-party monitoring tools available that can be used to monitor TPS and other performance metrics in AWS. Some popular options include New Relic, Datadog, and AppDynamics.

- RDS

Amazon RDS는 DB 인스턴스가 실행되는 운영 체제(OS)에 대한 측정치를 실시간으로 제공합니다. RDS는 지표를 향상된 모니터링에서 Amazon CloudWatch Logs 계정으로 전달합니다. 다음 표에는 Amazon CloudWatch Logs에서 사용 가능한 OS 측정치가 나와 있습니다.

### 참고한 사이트

[https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_Monitoring-Available-OS-Metrics.html](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_Monitoring-Available-OS-Metrics.html)
