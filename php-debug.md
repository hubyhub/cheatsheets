# xDebug

1. Goto [https://xdebug.org/wizard.php](https://xdebug.org/wizard.php) and follow steps:
2. Paste phpinfo output
3. download the correct php_xdebug.dll e.g.: php_xdebug-2.6.0-7.2-vc15.dll
4. Move the downloaded file to C:\xampp\php\ext
5. Edit C:\xampp\php\php.ini and add the line<br>
	`zend_extension = C:\xampp\php\ext\php_xdebug-2.6.0-7.2-vc15.dll`
6. Restart the webserver
7. Follow [Setup-Guide for IntelliJ](https://www.jetbrains.com/help/idea/configuring-xdebug.html):


