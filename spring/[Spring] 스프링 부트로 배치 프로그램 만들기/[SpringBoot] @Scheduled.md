### Scheduled

```java
//1초 마다 실행
@Scheduled(fixedDelay=1000)
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//5초 마다 실행
@Scheduled(fixedDelay=5000)
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//10초 마다 실행
@Scheduled(fixedDelay=10000)
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//매일 5시에 실행
@Scheduled(cron="0 0 05 * * ?")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//매월 2일,20일 새벽2시에 실행
@Scheduled(cron="0 0 02 2,20 * ?")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//매월 마지막날 저녁 10시에 실행
@Scheduled(cron = "0 0 10 L * ?")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }

//1시간 마다 실행 ex) 01:00, 02:00, 03:00....
@Scheduled(cron = "0 0 0/1 * * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//매일 오후 18시마다 실행 ex) 18:00
@Scheduled(cron = "0 0 18 * * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }

//매일 오후 18시00분-18시55분 사이에 5분 간격으로 실행
@Scheduled(cron = "0 0/5 18 * * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }


//매일 오후 9시00분-9시55분, 18시00분-18시55분 사이에 5분 간격으로 실행
@Scheduled(cron = "0 0/5 9,18 * * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }

//매일 오후 9시00분-18시55분 사이에 5분 간격으로 실행
@Scheduled(cron = "0 0/5 9-18 * * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }

//매달 1일 00시에 실행
@Scheduled(cron = "0 0 0 1 * *")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");

    }

//매년 3월내 월-금요일 10시 30분에만 실행
@Scheduled(cron = "0 30 10 ? 3 MON-FRI")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }
//매달 첫째주, 셋째주에 실행Execute on the first and third weeks of the month.
@Scheduled(cron = "0 30 10 ? 3 MON-FRI")
    public void testScheduler(){
        System.out.println("스케줄링 테스트");
    }
```

# 참고한 사이트

[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lovemema&logNo=140200056062](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lovemema&logNo=140200056062)
