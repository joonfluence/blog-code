# 본문

### string date ( string $format [, int $timestamp ] )

정수형으로 주어지는 timestamp나, timestamp가 주어지지 않았을 경우에는 현재 로컬 시간을 사용하여, 주어진 포맷 문자열에 따라 형식화한 문자열을 반환합니다. 즉 timestamp는 선택적이고, 기본값은 `time()`의 값입니다. 지원되지 않는 포맷 문자는 그대로 출력됩니다.

```php
$dateString = date("Y-m-d", time());
echo $dateString;

# 결과) 2017-01-10

date("Y-m-d");
date(string $format, ?int $timestamp = null);
```

1. 일

- d : 일, 앞에 0이 붙는 2 숫자
- D : 요일 글자 표현, 3 문자
- j : 앞에 0이 붙지 않는 일
- l : (소문자 'L') 요일의 완전한 글자 표현
- N : 요일의 ISO-8601 숫자 표현 (PHP 5.1.0에서 추가)
- S : 일 영어 접미사, 2 문자
- w : 요일 숫자 표현
- z : 해당 연도 일차

2. 주

- W : ISO-8601 주차, 주는 월요일에 시작 (PHP 4.1.0에서 추가)

3. 월

- F : January나 March 같은 월의 완전한 글자 표현
- m : 0이 붙는 월 숫자 표현
- M : 월의 축약 글자 표현, 3문자
- n : 0이 붙지 않는 월 숫자 표현
- t : 주어진 월의 일 수

### strtotime()

주어진 날짜 형식의 문자열을 1970년 1월 1일 0시 부서 시작하는 유닉스 타임스탬프로 변환합니다. 두번째 인자가 주어지면 주어진 타임스탬프를 기준으로 계산되어 집니다. 날짜가 주어지지 않고 변화량만 주어지면 로컬 타임이 사용됩니다.  +1 day, +1 week 등이 사용될 수 있고, 음수값도 사용됩니다.

```php
$timestamp = strtotime("+1 week");
echo date("Y-m-d", $timestamp), "<br/>";

$timestamp = strtotime("2016-12-01 +1 week");
echo date("Y-m-d", $timestamp), "<br/>";
```

### mktime()

인자로 주어진 값(시,분,초,월,일,년)에 대응하는 타임스탬프를 반환합니다.

```php
$timestamp = mktime(0, 0, 0, 1, 1, 2017);
echo date('Y-m-d', $timestamp);
```

# 참고한 사이트

[https://offbyone.tistory.com/38](https://offbyone.tistory.com/38)
