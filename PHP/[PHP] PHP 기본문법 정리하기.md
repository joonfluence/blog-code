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

5. 함수의 사용
6. 변수 값 사용
7. 쿠키 값 사용
8. 파일 업로드

```php
bool move_uploaded_file (string filename, string destination);
move_uploaded_file($FILES['upfile']['tmp_name'],'./uploads_dir');
```
