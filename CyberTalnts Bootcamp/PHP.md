# What is PHP? 

PHP is one of the most common scripting languages used in web development. It is used in the biggest blogging system on the web (WordPress). It is also used in many software frameworks like CakePHP, Symfony, Laravel, etc.

PHP script starts with , the PHP file extension is .php 

e.g myfile.php

# PHP Variables & Functions 

## Variables 

PHP variables start with a dollar sign ($). There are two types of variables in PHP: User-defined variables and Built-in variables.

### User-defined Variables 

![](https://lh5.googleusercontent.com/-ncDPn7r2OoHGX-IY1uI_sNBdaihYDWE2Ok6-rPxT4sjt-tetBkaf7JwXBXfaU1eyJ9DbQiXDWWilBeYunvrMrJxbwZ2fmlybheWXCreFI8REqWTVUWr8TrSNiwdwyNdPdgxoJOQLHKMLcIYvg)

### Built-in Variables 

They are reserved variables in PHP which can be accessed from anywhere (e.g inside function , class or file). They are also known as PHP superglobals.

- $GLOBALS
- $_SERVER
- $_REQUEST
- $_POST
- $_GET
- $_FILES
- $_ENV
- $_COOKIE
- $_SESSION
![](https://lh4.googleusercontent.com/QaIg8d7PDtjuYpjiVLbYDOILDhL6dgJ4EKe9XsFq2RYXvqYoPxV6hJjvhrs5976Nr-FPlye0GacS672Hy0QSw8DlF8h6kjgf3ENgPiowHpZHFevKcTP8XN098n1tKoUfxJC9BMq0sEuKmaNTwA)

The above code snippet is an example of using PHP built-in variables. 

Inside the HTML form tag, you will notice the action attribute value is

|   |
|---|
|echo $_SERVER['PHP_SELF'];?>|

echo: is used to print a message.

$_SERVER[‘PHP_SELF’]: is a built-in variable. Its value will be the current php file name.

|   |
|---|
|$_SERVER["REQUEST_METHOD"] // this will get the HTTP request method .|

|   |
|---|
|$_REQUEST['fname'] // this will get the value of the fname parameter|

Another way to get the GET and POST parameter value is:

|   |
|---|
|$_GET['fname']; // retrieves fname param value from the GET request <br><br>$_POST['fname']; // retrieves fname param value from the POST request|

## Functions 

There are two types: User-defined functions and Built-in functions. 

### User-defined Functions 

To declare a function, you need to write the following: 

|   |
|---|
|function functionName() {<br><br>   code to be executed;<br><br> }|

![](https://lh4.googleusercontent.com/DBe7iYvx2ORCith3-Cmm3vukIUlEeq2e44nUXXvcP2dPOiQi9LRhaS1FGKQ1u3nMfXkW9GOXahZ45Unqyaz_oYiTR6sqgkwPFeXF5A0-lXPFh6Xj8xL2mm30jkbRrwmZysjQ3eetHp92pD-Rrg)

To call the function, you just need to write the function name with ( ).

 ![](https://lh4.googleusercontent.com/wqAzjAsZ-ZU3pkUKJ2Wt9wc3MqN619rSbuCOE0DCQwrw0f3YHDOWOiwkqVFWqteMrScRJFm-6doJh3xW_VYyja9HprGAjsdXTViCcqgZkfcjLdXljH5wwHwmiITB8_9jorenoBsdxxPeeL7lEA)

### Built-in Functions 

They are reserved functions that can be used anywhere in the php script.

To use these functions, you only need to call them by their name with (), and provide the call with the specified arguments for each function.

#### Example: 

|   |
|---|
|echo empty($name); <br><br>//empty function takes a variable and returns true if the variable is //empty and false if not.|

You can check different examples [here](https://www.w3schools.com/php/php_ref_overview.asp).

# PHP Cookies and Sessions 

Cookies are the small pieces of data that are stored on the user's machine to allow the application server to identify him every time he uses the application resources.

Sessions are a little bit different, they are stored on the server-side with the ability to store objects, unlike the normal cookies which only store strings.

## Cookie

To set a cookie, you need to use:

[Setcookie](https://www.php.net/manual/en/function.setcookie.php) (cookieName, cookieValue, expirationDate, path) function.

|   |
|---|
|setcookie("page_cookie", "this is the cookie value", time() + (86400 * 30), "/");              // 86400 = 1 day|

To get the cookie value:

|   |
|---|
|echo $_COOKIE['page_cookie']; // print the page_cookie value.|

## Session

To use php sessions, you will need to call session_start() at the beginning of the php file.

|   |
|---|
|// Start the session<br><br> session_start();<br><br> ?>|

And to set a session:

|   |
|---|
|$_SESSION['username'] = "khaled";|

This will create a session variable with the value “khaled”.

And to access this session in the php script, you can use the session name.

|   |
|---|
|echo $_SESSION['username'] ;|

To remove all the session variables and destroy them, you can use session_unset() and session_destroy().

Check out this [tutorial](https://www.w3schools.com/php/default.asp).