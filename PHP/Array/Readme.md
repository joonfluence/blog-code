### array_diff

array_diff(배열1, 배열2)
반환값 - 배열1 값중 배열2에 없는 값만 배열 형태로 반환.

```php
$arrtmp1 = ['apple', 'orange', 'melon', 'banana', 'pineapple'];
$arrtmp2 = ['apple', 'orange', 'melon', 'grape'];

//array_diff함수를 사용 배열을 비교
$arrtmp_diff = array_diff($arrtmp1, $arrtmp2);

foreach($arrtmp_diff as $value){
	echo $value;
	echo '<br>';
}
```

### array_intersect

```php
 //example 1
$arr1 = array( 'a', 'b', 'c' );
$arr2 = array( 'c', 'd' );
$final = array_intersect( $arr1, $arr2 );

// output
$final = array( 'c' );
```
