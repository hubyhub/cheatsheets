# Node.js Express4 Boilerplate

 1. [Setup](#setup) 
 1. [Unit Tests](#unit-test) 
 

<a name="setup"></a>
##  Setup
1. Init npm

	```
	npm init
	```

2. adapt package.json 
	```
	"private": true,  	//prevents accidental publication of private repositories.
	```

3. Install Express & and Bodyparser

	```
	npm install express --save 		// the HTTP-server
	npm install body-parser --save  // simplifies parsing the content in the body in POST-requests)	
	```

4. create file for server (in windows command line)

	```
	copy NUL > server.js
	```

	...and add server-code:
	```
	const express = require('express');
	const path = require('path');
	const app = express();

	app.use(express.static( path.join(__dirname, 'public')));

	app.get('/', function(req, res) {
		res.sendfile('index.html');	
	});

	app.get('/user', function (req, res) {
	  res.send('Hello User');
	});

	app.post('/user', function (req, res) {
	  res.send('Got a POST request');
	});

	//Respond to a PUT request to the /user route:
	app.put('/user', function (req, res) {
	  res.send('Got a PUT request at /user');
	});

	//Respond to a DELETE request to the /user route:
	app.delete('/user', function (req, res) {
	  res.send('Got a DELETE request at /user');
	});

	app.listen(3000, function () {
	  console.log('Example app listening on http://127.0.0.1:3000')
	});
	```
	
<a name="unit-test"></a>
## Unit Testing

1. Install Mocha
	```
	npm install mocha --save-dev
	```

2. Add this to package.json 

	```
	"test": "mocha --reporter list"
	```

3. Create a /test directory (mochas default directory for unit-tests)
	```
	mkdir test   
	```

4. Create a file for the tests (in "/test")#

	```
	copy NUL > test.js
	```

5. Insert tests

	```
	var assert = require("assert");

	describe('project successfully started', function() {
	  it('should have a defined myVar', function() {
		assert.notEqual('undefined', MyProject.myVar);
	  }),

	  it('should be able to do xyz', function() {    
		var myObj = new BigObject(0);
		assert.equal(0, myObj.x);
	  })
	})
	```

6. Run the tests

	```
	npm test
	```