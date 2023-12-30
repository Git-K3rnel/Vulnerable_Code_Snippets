```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $password = $_POST["password"];
    
    // Retrieve user details from the database
    $sql = "SELECT id,email FROM users WHERE username = '$username'";
    $result = $conn->query($sql);
    $row = $result->fetch_array(MYSQLI_ASSOC);
    
    if ($row){
        echo $row['email'];
        echo "<br>";
        echo $row['id'];
    }
    else {
        echo 'failed';
    };
    
    // Free the result set
    $result->free_result();
}
?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```sql
a' union select group_concat(schema_name),2 from information_schema.schemata-- -
a' union select group_concat(table_name),2 from information_schema.tables where table_schema = database()-- -

  ```
</details>

<details>
  <summary><b>Fix with prepared statement</b></summary>

  ```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $password = $_POST["password"];
    
    // Prepare and execute the SQL query to retrieve user details from the database
    $stmt = $conn->prepare("SELECT password FROM users WHERE username = ?");
    $stmt->bind_param("s", $username);
    $stmt->execute();
    
    // Get the hashed password from the query result
    $stmt->bind_result($hashedPassword);
    $stmt->fetch();
    
    // Verify the password
    if (password_verify($password, $hashedPassword)) {
        // Password is correct, user is logged in
        echo "Login successful";
    } else {
        // Password is incorrect
        echo "Login failed";
    }
    
    // Close the statement
    $stmt->close();
}
?>
  ```
</details>
