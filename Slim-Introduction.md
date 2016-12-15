# Slim Introduction

 1. [Installation](#installation)
 

<a name="installation"></a>
###  1. Installation
1. Create a folder. e.g.: "dev/slim-intro"
2. Install composer by going to their [website](https://getcomposer.org/download/), copy the command-line code and paste it in the terminal.
3. A composer.phar file is now in your folder. Composer is now installed locally in the folder. (not computer-wide).
4. Install Slim by typing following code in the command-line<br>
	
	```php
	php composer.phar require slim/slim "^3.6"
	```
	That will create a **composer.json** in the root folder,<br> 
	and a **vendor/** folder, and download all the dependencies in that folder<br>
	*(Another possibility is to install the skeleton from the [Slim-webite](https://www.slimframework.com/))*
6. In the root folder create a folder **/public** and a index.php
7. Paste the HelloWorld code from [Slim-webite](https://www.slimframework.com/) in the index.php 
	```php
	<?php
	use \Psr\Http\Message\ServerRequestInterface as Request;
	use \Psr\Http\Message\ResponseInterface as Response;

	require '../vendor/autoload.php';

	$app = new \Slim\App;
	$app->get('/hello/{name}', function (Request $request, Response $response) {
		$name = $request->getAttribute('name');
		$response->getBody()->write("Hello, $name");

		return $response;
	});
	$app->run();
	```
9. Get rid of ugly url e.g.: http://website.com/**index.php**/api/books ending in url and get nice REST-urls, by creating a **.htaccess**-file in the **/public** folder:
	
	```
	RewriteEngine On
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule ^ index.php [QSA,L]
	```
	Try "Save As" in the editor, if you use windows and cannot save the file.

10. Setup a **virtual host** to have a nice name instead of *localhost* in following (XAMPP) file:

	```
	xampp\apache\conf\extra\httpd-vhosts.conf
	```

	Point to the Folder, and set the server-name
	```
	<VirtualHost 127.0.0.1>    
		ServerName slimintro  
		DocumentRoot C:/xampp/htdocs/slimintro
	</VirtualHost>
	```
	Is only the htdocs folder allowed?
11. and add hostfile entry (C:\Windows\System32\drivers\etc)

	```
	127.0.0.1	slimintro
	```

12. If you have a different dev folder than under "htdocs" just create a "directory junction" (windows)
	
	```
	mklink /J slimintro C:\Data\dev\slim-intro\public
	```
13. Restart Xampp and open:  [http://slimintro/hello/name](http://slimintro/hello/name) in the browser


