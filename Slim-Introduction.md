# Slim 3.6 Introduction

 1. [Installation](#installation) 
 2. [Database Basics](#database-basics)
 3. [Routing System Basics](#routing-system)
 4. [Basic Database Interactions](#database-fetching)
 5. [Useful Links](#links)
 

<a name="installation"></a>
##  Installation
1. Create a folder. e.g.: "dev/slim-intro"
2. Install composer by going to their [website](https://getcomposer.org/download/), copy the command-line code and paste it in the terminal.

	```sh
	 php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
	 php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
	 php composer-setup.php
	 php -r "unlink('composer-setup.php');"
	```
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


<a name="database-basics"></a>
## Database Basics

**Creates table "books" ([phpmyadmin](http://localhost/phpmyadmin))**

```sql
CREATE TABLE IF NOT EXISTS `books` (
  `id` int(11) NOT NULL,
  `title` varchar(250) COLLATE utf32_unicode_ci NOT NULL,
  `author` varchar(250) COLLATE utf32_unicode_ci NOT NULL,
  `amazon_url` varchar(250) COLLATE utf32_unicode_ci NOT NULL
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf32 COLLATE=utf32_unicode_ci;

INSERT INTO `books` (`id`, `title`, `author`, `amazon_url`) VALUES
(1, 'The Four Hour Work Week', 'Tim Ferries', 'www.fourhour.com'),
(2, 'The Four Hour Work Week', 'Tim Ferries', 'www.fourhour.com'),
(3, 'Harry Potter', 'cool Author', 'www.amazon.de'),
(4, 'Herr der Ringe', 'Tolkien', 'www.asdf.de');
```

**Create app/api/dbconnect.php**
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db_name = "myslimsite";
$mysqli = new mysqli($host, $user, $pass, $db_name);
```

<a name="routing-system"></a>
## Routing System
Taking URLs and converting them into some action.

**GET**
```php

$app->get('/api/books', function (Request $request, Response $response) {    
    $response->getBody()->write("Hello, listing all books");
	return $response;
});

// get variables from URL
$app->get('/api/books/{id}', function($request, $response){    
    $id = $request->getAttribute("id");
    $response->getBody()->write("Hello id: $id");
	return $response;   
});

```

**POST**
```php
// in postman select "form-data" as content-type in the body tab
$app->post('/api/books', function($request){
	// $myName = $_POST('my_name');
	$myName = $request->getParsedBody()['my_name'];		// PSR-7 Way
	echo "hello ".$my_name;
});
```

**PUT**
```php
// in postman select "x-www-form-urlencoded" as content-type in the body tab
$app->put('/api/books', function($request){	
	$myName = $request->getParsedBody()['my_name'];
	echo "hello this is a put request with ".$my_name;
});
```

**DELETE**
```php
$app->delete('/api/books/{id}', function($request){	
	require_once('dbconnect.php');
	$id = $request->getAttribute('id');
	$query = "DELETE FROM books WHERE `books`.`id` = $id";
	$result = $mysqli->query($query);
});	
	
```


<a name="database-fetching"></a>
## Fetching Data from Database 
**books.php**
#### GET
```php
<?php
	// display all records
	$app->get('/api/books', function(){
		require_once('dbconnect.php');
		$query = "select * from books order by id";
		$result = $mysqli->query($query);
		
		while($row = $result->fetch_assoc()){
			$data[] = $row;
		}
		if(isset($data)){	
			header('Content-Type: application/json');
			echo json_encode($data);
		}
	});
	
	// display single row
	$app->get('/api/books/{id}', function($request){
		require_once('dbconnect.php');
		$id = $request->getAttribute("id");
	
		$query  = "select * from books where id=$id";
		$result = $mysqli->query($query);
		$data[] = $result->fetch_assoc();
		
		if(isset($data)){	
			header('Content-Type: application/json');
			echo json_encode($data);
		}	
	});
```

#### POST
```
// add a record, select "form-data" as content-type
$app->post('/api/books', function($request){
	require_once('dbconnect.php');	
	// Prepared Statement
	$query = "INSERT INTO 'books' ('title', 'author', 'amazon_url') VALUES (?,?,?)";
	$stmt = $mysqli->prepare($query);
	$stmt->bind_param("sss", $a, $b, $c);
	
	$a = $request->getParsedBody()['book_title'];   //"Book Title";
	$b = $request->getParsedBody()['author'];   //"Author Name";
	$c = $request->getParsedBody()['amazon_url'];   //"Author Name";
	
	$stmt->execute();
	echo done();
});
```

#### PUT
```php
// update a record. select "x-www-form-urlencoded" as content-type
$app->put('/api/books/{id}', function($request){	
	require_once('dbconnect.php');
	
	// Prepared Statement	
	$query = "UPDATE `books` SET `title` = ?,`author` = ?,`amazon_url` = ? WHERE `books`.`id` = $id";
	$stmt = $mysqli->prepare($query);
	$stmt->bind_param("sss", $a, $b, $c);
	
	$a = $request->getParsedBody()['book_title'];   //"Book Title";
	$b = $request->getParsedBody()['author'];   //"Author Name";
	$c = $request->getParsedBody()['amazon_url'];   //"Author Name";
	
	$stmt->execute();
	echo done();
});
```

#### DELETE
```	
// Delete a record / use "x-www-form-urlencoded" as content-type
$app->delete('/api/books/{id}', function($request){	
	require_once('dbconnect.php');
	$id = $request->getAttribute('id');
	$query = "DELETE FROM books WHERE `books`.`id` = $id";
	$result = $mysqli->query($query);
});	
	
	
```
<a id="links"></a>
## Useful Links
[http://stackoverflow.com/questions/15089294/slim-framework-for-beginners](http://stackoverflow.com/questions/15089294/slim-framework-for-beginners)<br>
[HTTP Basic Auth with SLIM 1](http://www.appelsiini.net/2014/slim-database-basic-authentication)<br>
[HTTP Basic Auth with SLIM 2](https://github.com/tuupola/slim-basic-auth)


