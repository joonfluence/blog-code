# 서론

오늘은 PHP 기본문법에 대해서 알아보도록 하겠습니다.

# 본론

1. PHP 태그 정의

```php
<?
  echo "hello world"
?>
```

<?로 시작해서 ?>로 마무리 짓습니다.

2. 연산자

- 산술 연산자
- 관계 연산자
- 논리 연산자

3. 조건문
4. 배열의 사용

```php
<?
    $eng_score = array(87, 76, 98, 87, 87, 93, 79, 85, 88, 63, 74, 84, 93, 89, 63, 99, 81, 70, 80, 95);
    $sum=0;
    for($a=0; $a<20; $a++){
         $sum = $sum + $eng_score[$a]; // 20명의 학생의 성적의 누적 합
    }
    $avg = $sum/20; // 평균 구하기
    echo "학생들 영어 점수 : ";
    for($a=0; $a<20; $a++){ // 입력된 학생들의 영어 성적 출력
     echo $eng_score[$a]." ";
    }
?>
```

배열을 서로 합치는 방법

```php
$a = array('a', 'b');
$b = array('c', 'd');
$merge = array_merge($a, $b);
// $merge is now equals to array('a','b','c','d');
```

5. 함수의 사용
6. 변수 값 사용
7. 쿠키 값 사용
8. 파일 업로드

```php
bool move_uploaded_file (string filename, string destination);
move_uploaded_file($FILES['upfile']['tmp_name'],'./uploads_dir');
```

```php
file_get_contents(string $filename, bool $use_include_path = false, ?resource $context= null, int $offset = 0,?int $length = null): string|false
```

This function is similar to file(), except that file_get_contents() returns the file in a string, starting at the specified offset up to length bytes. On failure, file_get_contents() will return false.

file_get_contents() is the preferred way to read the contents of a file into a string. It will use memory mapping techniques if supported by your OS to enhance performance.
file_get_contents can do a POST, create a context for that first:

```php
<?php

$opts = array('http' =>
  array(
    'method'  => 'POST',
    'header'  => "Content-Type: text/xml\r\n".
      "Authorization: Basic ".base64_encode("$https_user:$https_password")."\r\n",
    'content' => $body,
    'timeout' => 60
  )
);

$context  = stream_context_create($opts);
$url = 'https://'.$https_server;
$result = file_get_contents($url, false, $context, -1, 40000);

?>
```

# 참고한 사이트

[https://www.php.net/manual/en/function.file-get-contents.php](https://www.php.net/manual/en/function.file-get-contents.php)
[https://stackoverflow.com/questions/4268871/php-append-one-array-to-another-not-array-push-or](https://stackoverflow.com/questions/4268871/php-append-one-array-to-another-not-array-push-or)
