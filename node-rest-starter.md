npm init
// add in package.json "private": true,  This is a way to prevent accidental publication of private repositories.


# Install Server
npm install express --save 
npm install body-parser --save
copy NUL > server.js    // add server file

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






# Unit Testing
npm install mocha --save-dev
//add this to package.json "test": "mocha --reporter list"
mkdir test  // we need test directory. 
copy NUL > test.js


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

npm test