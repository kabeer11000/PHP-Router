# PHP Router
Because everyone wants simple Express Style Routing for PHP

More Documentation will be added Soon in the mean time stick with this!


## The Problem
```php
<?php

if(isset($_REQUEST['page_id'])){
	
    if($_REQUEST['page_id']==3){
		// load page
	}
 
	if($_REQUEST['action']=='delete'){
		// do something
	}
 
	// do more complex things
	
}

```
This method quickly becomes confusing and you lose yourself in parameters and if loops. The result is spagehtti code. In addition, the URLs are difficult to understand and not easy to remember.


## The Solution
- PHP Router Library

```php
<?php

// Include router class
include('Route.php');


// Add base route (startpage)
Route::add('/',function(){
    echo 'Welcome :-)';
});


// Simple test route that simulates static html file
Route::add('/test.html',function(){
    echo 'Hello from test.html';
});


// Post route example
Route::add('/contact-form',function(){
    echo '<form method="post"><input type="text" name="test" /><input type="submit" value="send" /></form>';
},'get');

// Post route example
Route::add('/contact-form',function(){
    echo 'Hey! The form has been sent:<br/>';
    print_r($_POST);
},'post');

// Accept only numbers as parameter. Other characters will result in a 404 error
Route::add('/foo/([0-9]*)/bar',function($var1){
    echo $var1.' is a great number!';
});

Route::run('/');

```
Any routes can be created here. The parameters can be parsed from the route via RegExp and passed to the handler. In addition, you can control through which methods (get, post, put, patch, etc ...) the routes may be accessed. The handlers gets only executed if the routes match the entered path.


## Finishing Up 
Put this in your apache servers ```.htaccess``` file to use PHP Router

```htaccess
DirectoryIndex index.php

# enable apache rewrite engine
RewriteEngine on

# set your rewrite base
# Edit this in your init method too if you script lives in a subfolder
RewriteBase /

# Deliver the folder or file directly if it exists on the server
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
 
# Push every request to index.php
RewriteRule ^(.*)$ index.php [QSA]
```
As you can see, the router consists of less code than you might think. Only if the run() method gets executed, the individual routes registered with add () will be checked and executed. When running, the base path in which the project lives in, is always placed before the pattern and thus taken into account. If no match is found, the 404 route or the 405 route is automatically triggered.

Of course all requests have to be forwarded to index.php with a rewrite.

Copyright 2020 Kabeers Network. Content Under MIT Licence
