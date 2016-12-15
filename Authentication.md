# Authentication
## HTTP-Basic Authentication (Basisauthentifizierung) 
Der Client sendet die Authentifizierung mit dem Authorization-Header in der Form **Benutzername:Passwort** Base64-codiert an den Server.

**Beispiel** <br>
Teil eines Request-Headers:
```
Authorization: Basic d2lraTpwZWRpYQ==
```
**"Basic"** zeigt an, dass es sich um eine Basic Authentifizierung handelt.<br>
**"d2lraTpwZWRpYQ=="** ist hier nur Base64-codiert. Passwort und Username können gewonnen werden.<br>
* Ein Nachteil ist, dass Benutzername und Passwort nicht verschlüsselt werden.
Es ist daher genauso unsicher als würde das Passwort im Klartext übertragen werden.
* HTTS (SSL/TLS) ist dahier ein MUSS!
* Weiters werden U und P mit jedem Request gesendet!

## OAuth1
is an open standard for TOKEN-BASED Authentication.<br> 
It allows users to access server resources, without providing their credentials with each request.
This provides enhanced security over HTTP-Basic Authentication.

**Request-Headers:**<br>
```
Authorization: Bearer 8u_209iul23k4oaspdfyx823928u_209iua0
```
* **"Bearer"** zeigt an, dass es sich um einen TOKEN handelt<br>
* **8u_209iul23k4oaspdfyx823928u_209iua0** ist der TOKEN. Beinhaltet NICHT PWD oder Username.

**Refresh Token**: allows us to request for a new access token. <br>
Usually it has a  longer lifespan (e.g.: 100 days)<br>
**Access Token**: is sent out with each request. Changing it frequently reduces the chance of a MITM attack. Lifespan is usually short(e.g.30min)
			  a longer lifespan would diminish oauth1 advantage over basic auth.<br>
**Client Secret**: is not the password, but it allows you to generate access-tokens, that authenticate you as a particular user. (should be protected as a password)

We use the access-token for its lifespan and then use the refresh-token to get a new acces-token.

you can do a login for example be doing a POST-request to **"https://dummydomain.xyz/oauth_token.do"**
by providing following parameters in the body of a "x-www-form-urlencoded" request:
* grant_type : password
* client_id : 234c322jalk33
* client_secret: abc123
* username : asdfk.oauth1
* password : mysupersecretpassword

the response would be a **200 OK** and a **access-token** and a **refresh-token**

In order to refresh the access-token you might do another request to the server: **"https://dummydomain.xyz/oauth_token.do"**
This time with:
grant_type : refresh_token
client_id  : 234c322jalk33  	// stays the same
client_secret : abc123  		// stays the same
refresh_token : 23498asdf9a9df8 // the one that we got in the last response

Response might be a new access-token:
```javascript
{
	"scope" : useraccount,
	"token_type" : "Bearer",
	"expires_in" : 1800,  			// seconds
	"refresh_token" : "23498asdf9a9df8",
	"access_token" : "23423asdf"  	// new access-token that can be used for next 30min ()( 
}
```







## OAUth2