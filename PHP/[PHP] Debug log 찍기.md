### Apache 로그에 찍어보기

```php
file_put_contents('php://stderr', print_r($foo, TRUE))
```

Apache 에러 로그를 찍어볼 수 있다.

### 디버깅 로그 출력하기

```php
error_log ("hello world", 3, "/home/user/logs/debug.log");
```

```php
<?php
    $arr = array(1, 3.14, 'string', '', ' ', null, true, false);

    echo '[ var_dump ]<br>';
    var_dump($arr);

    echo '<br><br>[ var_export ]<br>';
    var_export($arr);

    echo '<br><br>[ print_r ]<br>';
    print_r($arr);
?>

```

- var_dump: 배열의 값들의 자료형까지 보여준다. 따라서 자료형을 확인하는 목적이면 var_dump를 사용한다. 단점은 가독성이 떨어져서 어떤 key에 어떤 value가 들어가 있는지 한 눈에 확인하기 어렵다.

```plaintext
[ var_dump ]
array(8) { [0]=> int(1) [1]=> float(3.14) [2]=> string(6) "string" [3]=> string(0) "" [4]=> string(1) " " [5]=> NULL [6]=> bool(true) [7]=> bool(false) }
```

- var_export : 자료형과 간결성을 모두 갖춘 형태이다. var_dump의 결과값에서 자료형 부분만을 없앤 값과 같다. 예를 들어 var_dump의 int(1)는 var_export에서는 그냥 1을 출력한다. var_dump의 string(6) "string"은 var_export에서는 그냥 'string'을 출력한다. null과 true, false 모두 그냥 NULL, ture, false로 출력됨을 확인하자.

```plaintext
[ var_export ]
array ( 0 => 1, 1 => 3.14, 2 => 'string', 3 => '', 4 => ' ', 5 => NULL, 6 => true, 7 => false... )
```

- print_r : 배열의 값들의 실제 출력값 형태로 보여준다. 따라서 자료형에 상관없이, 그냥 출력값을 확인하는 목적으로는 print_r를 사용한다. 단점은 빈칸과 null, true, false 등은 전부 빈칸으로 출력되서 자료형을 구분하지 못한다.

```plaintext
[ print_r ]
Array ( [0] => 1 [1] => 3.14 [2] => string [3] => [4] => [5] => [6] => 1 [7] => )
```
