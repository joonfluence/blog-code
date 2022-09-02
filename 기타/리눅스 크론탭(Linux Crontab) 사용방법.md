### 정의

크론탭은 특정 시간에 특정 작업을 해야할 때, 사용합니다.

### 크론탭 기본

```
crontab -e
```

위 명령어를 입력하면, 크론탭을 설정할 수 있는 파일이 나옵니다. 여기에 각종 크론탭 명령어를 입력 후, 콜론(:) 입력 후에 wq 를 입력해 크론탭을 갱신시킵니다.

```
crontab -l
```

위 명령어를 입력하면, 크론탭 파일을 읽어옵니다.

```
crontab -r
```

크론탭을 지울 수 있습니다.

```
* * * * * ls -al
```

별이 다섯개나 있습니다. 그리고 뒤에는 명령어가 적혀 있네요. 이게 기본 사용법입니다. 물론 쉘스크립트 뿐만 아니라 리눅스 커맨드도 사용할 수 있습니다. 여기서는 쉘스크립트를 사용하는 방법으로 설명하고 있습니다.
별이 다섯개 있는 경우엔 "매분마다 실행" 하는겁니다. 별이 지칭하는 것이 무엇인지 자세히 살펴봅시다.

### 주기 결정

\*　　　　　　\*　　　　　　\*　　　　　　\*　　　　　　\*
분(0-59)　　시간(0-23)　　일(1-31)　　월(1-12)　　　요일(0-7)

### 주기별 예제

- 매분 실행

```
* * * * * /home/script/test.sh
```

- 특정 시간 실행

```shell
# 매주 금요일 오전 5시 45분에 test.sh를 실행
45 5 * * 5 /home/script/test.sh
```

- 반복 실행

```shell
# 매일 매시간 0분, 20분, 40분에 test.sh를 실행
0,20,40 * * * * /home/script/test.sh
```

- 범위 실행

```shell
# 매일 1시 0분부터 30분까지 매분 test.sh를 실행
0-30 1 * * * /home/script/test.sh
```

- 간격 실행

```shell
# 매 10분마다 test.sh를 실행
*/10 * * * * /home/script/test.sh
```

- 조금 복잡하게 실행

```shell
# 5일에서 6일까지 2시,3시,4시에 매 10분마다 test.sh를 실행
*/10 * * * * /home/script/test.sh
```

- 매월 말일 실행

```shell
echo $(date -d '+1 day' +%d) # 2일이면, 03으로 출력됨
0 0 28-31 * * /usr/bin/test $(date -d '+1 day' +%d) -eq 1 && 실행하고 싶은 커맨드
```

마지막 달은 일정하지가 않기 때문에 crontab에 설정을 할 때에 test 커맨드와 date 커맨드를 사용해 마지막 날에만 실행할 수 있는 스케줄을 작성해줍니다.

### 크론 사용팁

- 한 줄에 하나의 명령만 씁니다.

- 주석을 달아봅시다.

```shell
# 주석 #
#--------------------#
# 이것은 주석입니다. #
#--------------------#
```

- 변경 후, 반드시 cron 다시 실행시키기

```shell
service cron start  # (가동) or
service cron restart # (재가동)
```

- cron 프로그램이 돌고 있는지 확인하자.

```shell
service cron status
```

- cron 실행중 출력한 메세지는 /var/mail 에서 확인할 수 있다.

```shell
cat /var/mail/root
```

# 참고한 사이트

[https://jdm.kr/blog/2](https://jdm.kr/blog/2)
[https://happist.com/558036/%EC%9B%B9%EC%84%9C%EB%B2%84-%EC%9E%90%EB%8F%99-%EC%8B%A4%ED%96%89-crontab%ED%81%AC%EB%A1%A0%ED%83%AD-%EC%A0%81%EC%9A%A9-%EC%8B%9C-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9](https://happist.com/558036/%EC%9B%B9%EC%84%9C%EB%B2%84-%EC%9E%90%EB%8F%99-%EC%8B%A4%ED%96%89-crontab%ED%81%AC%EB%A1%A0%ED%83%AD-%EC%A0%81%EC%9A%A9-%EC%8B%9C-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9)
