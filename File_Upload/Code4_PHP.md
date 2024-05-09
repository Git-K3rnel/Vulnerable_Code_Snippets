```php
<?php
function uploadImage(){
        $result = true;

        if (!isset($_POST['submitBtn'])){
            $this->showUploadForm();
        } else {
            $msg = '';
            $error = '';

            //Check image type. Only jpeg images are allowed
            if ( (($_FILES['myfile']['type'])=='image/pjpeg') || (($_FILES['myfile']['type'])=='image/jpeg')) {

               // Check the output directories
               if ($this->checkDirs()){
                   $target_path = $this->originalDir . basename( $_FILES['myfile']['name']);

                   if(@move_uploaded_file($_FILES['myfile']['tmp_name'], $target_path)) {
                      $msg = basename( $_FILES['myfile']['name']).
                      " (".filesize($target_path)." bytes) was stored!";
                   } else{
                      $error = "The upload process failed!";
                      $result = false;
                   }
                }
            } else {
               echo "Only jpeg images are allowed!";
            }
            $this->showUploadForm($msg,$error);
        }
    }
```

<details>
  <summary><b>Exploite</b></summary>

  ```bash
  # only checks file type
  # upload a php revshell with .jpeg format and intercept the traffic in burp
  # change the .jpeg to .php and send the request
  ```
</details>
