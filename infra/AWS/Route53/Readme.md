# 서론

# 본론

1. 등록대행자(한국에선 가비아 같은 업체)를 통해, 등록소로부터 도메인을 구매할 수 있다.
2. 도메인 주소를 가진 IP 주소와 연결해준다.
3. 네트워크에 연결되면, 주소 값은 Root name server를 통해 해당 IP 주소를 찾는다.

### Route 53의 역할

1. 등록대행자를 임대해준다.
2. DNS(Name Server)를 임대해준다.

## Route 53 활용 방법

### 도메인을 AWS에서 구매하는 경우

도메인이 없는 경우, Route 53을 등록대행자로 사용할 수 있다. Register Domain을 클릭해, 아무도 쓰지 않는 도메인만 등록할 수 있다.

### 이미 도메인을 갖고 있는 경우

이미 도메인을 갖은 경우, Route 53를 네임 서버로 활용할 수 있다.

### 하나의 IP 주소로부터 여러 개의 서비스를 실행하는 경우

CNAME 레코드를 추가해줄 수 있다.
정식 이름 또는 CNAME 레코드는 실제 또는 정식 도메인 이름에 별칭 이름을 매핑하는 DNS 레코드 유형입니다.
CNAME 레코드는 일반적으로 www 또는 mail과 같은 하위 도메인과 이 하위 도메인의 콘텐츠를 호스팅하는 도메인을 매핑하는 데 사용됩니다.