# 서론

오늘은 PHP 서버에서 간단하게 이미지 업로드를 하는 방법에 대해 알아보도록 하겠습니다. 이 때 프론트엔드 서버와 백엔드 서버가 분리되었음을 가정하고 진행하겠습니다.

### 프론트엔드

```html
<form method="post" enctype="multipart/form-data" action="upload.php">
  <input type="file" name="upload_file" onChange="{handleUpload}" />
  <input type="submit" value="업로드" />
</form>
```

```javascript
  const handleUpload = (e: any) => {
    const formData = new FormData();
    formData.append('file', e.target.files[0]);

    yield fetch('http://127.0.0.1:8000/upload.php', {
    method: 'POST',
    body: formData,
    });
  };
```

formData 형태의 값을 백엔드로 전달해줘야 합니다. 

### 백엔드

1. 서버실행

```
php -S 127.0.0.1:8000
```

2. 파일접근

- fileName : $\_FILES["file"]["name"];

전송한 파일에 접근하려면 초전역변수인 `$\_FILES`에 접근해야 합니다. 그리고 2번째 배열에 적은 값에 따라, (여기서는) input name에 해당 하는 값을 입력해줘야 합니다.

3. 업로드 기능 수행

- bool move_uploaded_file(string $filename, string $destination)

PHP에는 파일을 업로드하는 내장함수 `move_uploaded_file`가 존재합니다. 첫번쨰 인자로 파일명, 두번째 인자로 업로드될 경로를 입력해줍니다.

```php
// upload.php

<?php
header('Content-Type: application/json; charset=utf-8');
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: PUT, GET, POST");

$response = array();
$DIR = 'uploads/';
$urlServer = 'http://127.0.0.1:8000';

if($_FILES['file'])
{
    $fileName = $_FILES["file"]["name"];
    $tempFileName = $_FILES["file"]["tmp_name"];
    $error = $_FILES["file"]["error"];

    if($error > 0) {
        $response = array(
            "status" => "error",
            "error" => true,
            "message" => "Error uploading the file!"
        );
    } else {
        $FILE_NAME = rand(10, 1000000)."-".$fileName;
        $UPLOAD_IMG_NAME = $DIR.strtolower($FILE_NAME);

        $UPLOAD_IMG_NAME = preg_replace('/\s+/', '-', $UPLOAD_IMG_NAME);

        if(move_uploaded_file($tempFileName , $UPLOAD_IMG_NAME)) {
            $response = array(
                "status" => "success",
                "error" => false,
                "message" => "Image has uploaded",
                "url" => $urlServer."/".$UPLOAD_IMG_NAME
              );
        } else {
            $response = array(
                "status" => "error",
                "error" => true,
                "message" => "Error occured"
            );
        }
    }
} else {
    $response = array(
        "status" => "error",
        "error" => true,
        "message" => "File not found"
    );
}

echo json_encode($response);
?>
```

### 마무리

이상으로 간단하게 리액트 서버에서 PHP 서버로 이미지를 업로드하는 방법에 관해 간단하게 알아보았습니다. 읽어주셔서 감사합니다. 
