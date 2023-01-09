### 서론

오늘은 RDS의 다중 AZ 설정에 관해서 알아보도록 하겠습니다.

### 작동방식

Amazon RDS 다중 AZ 배포에서 Amazon RDS는 자동으로 프라이머리 데이터베이스 DB 인스턴스를 생성하고 동시에 다른 AZ의 인스턴스에 데이터를 복제합니다. 장애를 감지하면 Amazon RDS는 수동 개입 없이 자동으로 대기 인스턴스로 장애 조치합니다.
대기 중인 인스턴스의 갯수는 1개, 2개에서 선택할 수 있습니다.

### 비용

RDS 인스턴스를 사용하는 비용에 2배에서 3배 정도 요금이 부과됩니다. 그 이유는 1대 혹은 2대의 인스턴스가 대기중인 상태로 추가되기 때문이라고 생각됩니다. 
단일 AZ 배포, 대기 인스턴스가 1개인 다중 AZ 배포 및 읽기 가능한 대기가 2개인 다중 AZ 배포의 경우 요금은 `DB 인스턴스가 시작된 시간부터 중지되거나 삭제될 때까지 소비된 DB 인스턴스 시간당`입니다. 부분적으로 사용된 DB 인스턴스 사용 시간 요금은 DB 인스턴스 클래스 생성, 시작 또는 수정 같은 청구 가능한 상태 변경에 따라 1초 단위로 청구되며 최소 10분의 요금이 부과됩니다. 

- 다중 AZ 스토리지 요금 
  - 월별 GB당 0.30 USD
- 다중 AZ 프로비저닝된 IOPS 요금
  - 월별 IOPS당 0.24 USD

### 참고한 사이트

[https://aws.amazon.com/ko/rds/features/multi-az/](https://aws.amazon.com/ko/rds/features/multi-az/)