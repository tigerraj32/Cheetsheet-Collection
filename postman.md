# Dynamic Variable

## Setting dynamic variable via external script.

Postman can only trigger HTTP request, So there is no direct way we can execute script such as **.sh script**. 
But you can run a local server (here node js server) and write a node js script to execute a **shell script** or other executable file
Now let do that In step by step

### Step 1: Define Your Task
I hav a php script that provide me the token. This token will be later use in api request. This script looks some thing 
like this whichis saved as **TokenGenerato.php** 

```php
<?php
require "vendor/autoload.php";

class TokenGeneration {

	public  $appID= "50A454F3-EE11-4A4C-B708-965BC7640C08";
	public $token = NULL; 

	public function __construct(){
		//echo "\n URL: ".$this->url;
		$this->token = $this->generateAppToken();
	}
  
	function generateAppToken(){
		$message =   $this->appID."~REQUEST".time();
		//echo "\n message: ".$message;

		$cryptor = new \RNCryptor\RNCryptor\Encryptor;
		$base64Encrypted = $cryptor->encrypt($message, $this->appID);
		//echo "\n token: ". $base64Encrypted;
		return $base64Encrypted;
	}
}
?>
```

### Step 2: Get your token
We will write **token.php** program to get token from  **TokenGenerato.php** 

```php
<?php
  require "TokenGeneration.php";
  $tokengen = new TokenGeneration();
  echo $tokengen->token;
?>
```

we can run this php script via 
> php token.php

Output:
> AwFmHNkA1HdM1VHFadu89nE3xZuKO3pLQ7cHOrCj2x2WZoSt                                                                                                                                                                    


### Step 3: Create shell script to copy token into clipboard.
Mac provide some execlusive command such as **pbcopy** and **pbpaste** to copy the standard input into clipboard. Now create 
**token.sh** script with following code

```sh 
php /Volumes/Product/Research/uatci/token.php | pbcopy
```
make the script executable.
> chmod +x token.sh

[Refer here for other platform](https://www.ostechnix.com/how-to-use-pbcopy-and-pbpaste-commands-on-linux/) for similar command like **pbcopy**

### Step 4: Make the Shell script accessible globally

Create a symbolic link to token.sh in **/usr/local/bin/**  so that you can directly access this script from anywhere.

> ln -s /your-directory-path/token.sh /usr/local/bin/token

After this we can run following command to get token from out php script
> token

output: 
> AwFmHNkA1HdM1VHFadu89nE3xZuKO3pLQ7cHOrCj2x2WZoSt 


### Step 5: Pre-Request Script for Postman.



