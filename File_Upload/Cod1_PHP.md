```php
<?php

$uploaddir = '/var/www/html/uploads1/';
$uploadfile = $uploaddir . basename($_FILES['upfile']['name']);

echo "<p>";

if (move_uploaded_file($_FILES['upfile']['tmp_name'], $uploadfile)) {
  echo "File is valid, and was successfully uploaded.\n";
} else {
   echo "Upload failed";
}

echo "</p>";
echo '<pre>';
echo 'Here is some more debugging info:';
print_r($_FILES);
print "</pre>";

?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```bash
- there is no check here
- create a file with `.php` and upload it
  ```
</details>
