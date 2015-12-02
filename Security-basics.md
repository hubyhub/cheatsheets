# OWASP: 10 most common security threads (owasp.org)


01. Injection Flaws
02. Authentication
03. XSS- Cross Site Scripting
04. Insecure Direct Object References
05. Security Misconfiguration
06. Sensitive Data Exposure
07. Missing Function Level Access Control
08. Cross Site Request Forgery
09. Using Known Vulnerable Components
10. Unvalidated Redirects and Forwards


## 1. (SQL) Injection

When an attacker inserts extra data into a conversation.
This extra data allows the attacker to do malicious actions.

Its the point where the app parses the messages, where that injection occurs.
Since there are many places where messages are parsed, there are many places where injections can happen.
App can be attacked by:
 -> LDAP Injection (the attacker targets LDAP directories)
 -> XML and XPATH Injection (where the target is the XML parser)
 -> Log Forging (where the attacker injects arbitrary log entries)
 -> OS Command Injection (malicious inputs to trigger OS commands)
 -> XSS  (untrusted JavaScript Code is injected into a conversation)
 -> SQL-Injection (THE NUMBER 1- most dangerous)
 
 HOW DOES SQL-INJECTION WORK?
 --> Form-field allows user to input data
 --> when the user/attacker submits the data it is sent to the server that is hosting the application
 --> input-data is usually stored in a database
 --> instead of normal data, the hacker inputs SQL-commands (and therefore can list data, modify or even delete data)
 
 HOW TO PROTECT FROM SQL INJECTIONS:
 --> Proper use of "Parametrized Queries": is a Query that defines place-holder values "Bind Variables"
		Bind Variables work, because they force the SQL Parser to interpret these variables 
		as data and not as extra SQL commands. Parametrized Queries are supported by PHP, .Net, Java,...
 
 EXAMPLE JAVA: 
 PreparedStatement stmt = connection.prepareStatement("SELECT * FROM users WHERE userid=? AND password=?") ;
 stmt.setString(1,userid);
 stmt.setString(2,password);
 
 NOTE:
 Filtering out certain characters like "single quote, slash,..) DOES NOT prevent SQL injection!
 Attackers can simply use other techniques that don't rely on single quote character.
 Filtering Single quotes would also cause problems for anyone with that character in their name: "O'Connor"
 
 SQL Injection is so wide spread, because its easy to misuse these APIs to create vulnerable code.
 e.g.: this JAVA-CODE is not safe:
 String query = "SELECT * FROM users WHERE userid='"+userid+"'"+ " AND password='"+ password+"'";
 The SQL-Statement is created dynamically, by concatenating strings ( + + + )
 
 USE "BIND VARIABLES"!!!
 However some other technologies don't support Parametrized Queries.
 There the attacks need to be prevented using other techniques, e.g.: "encoding"  for preventing XSS
 
 
 
## 2a. Authentication  

 Authentication: is the process of requiring someone to prove that they are who they claim to be:
 This can be done in many ways:
 --> Username+Password
 --> Client-Side Certificates
 --> One-time Passwords
 Most important thing to-do: Authentication Systems should not reveal more than necessary.
 Most common errors, is "revelation of user details".  Protect username!
 
 HOW DOES IT WORK? How can attacker find usernames?
 1) "Login"-page:
 Don't use different error-messages for "invalid username" and "valid username but wrong password".
 Everything needs to be the same: the url, the content including the error-message
 Otherwise attacker can find out valid usernames.
 2) "Reset Password"-page!
  Allow the use to enter his email-adress/username, and after submitting show him something like:
  "thank you, email has been sent. if you havent gotten email, please check junk folder, and verify correct
  username";
 3) "New Account"-Page 
 --> Provide a list of possible choices with random additions based on the original request of the user.
 --> Meaning: add some random numbers at the end "manfred_123", "manfred_81" 
 --> again: always same error-messages
 
 Password Protection:
 --> protect Password through encryption (TLS) obfuscation *****
 --> Network-must be encrypted
 --> Password should never be stored in clear-text. 
 --> Storing Hash of password (and not password directly) is a first step, but not enough.
 --> hashing: password is taken into a cryptographic algorithm, making it unrecognisable.
 --> use strong hashing function.
 
 4) "Log off"-Process
 --> errors here can lead to Cross Site Request Forgery, which takes advantage of the fact, that the 
 authentication stays valid for hours, days, weeks or even forever.
 
 
## 2b. Session Management

*) protect sessionIds at all time
*) sessionId needs to be random and long enough
*) any time application changes state sessionid needs to change in order to prevent session-fixation attacks.

COOKIES are the most secure way for tracking sessions within the web-application


 While validating a users credentials is not difficult, session management is more of a challenge.
 A session is a unique instance of a use within our application.
 
 Why is this difficult?
 HTTP is stateless, that means from the servers view there is no relationship between successive queries
 even when they come from the same client.
 
 Creating a session requires that the application generate a unique token, that can be sent to the client, 
 and then submitted back to the application with each request.
 
 After authentication a unique token will be used to re-authenticate the user in every subsequent request,
 without requiring that they re-enter their credentials.
 Therefore Session management is essentially the foundation for access-control within our application.

 
 FLAWS IN SESSION-MANAGEMENT SYSTEMS
 Example: 
 1) User is interacting with web-application, 
 2) submits credentials through encrypted connection
 3) application sents back session-id in NON-encrypted connection
 4) Session-id can be sniffed via open Network.

 
 
 3 different Tracking mechanisms:
 URL REWRITING
 BASIC AUTHENTICATION
 COOKIES
 
 URL REWRITING: 
 --> means that the Session-id is embedded within the URL itself. 
 --> Works with every browser
 --> URL including Session-id is stored in Webserver access-log. If attacker gets access to webserver ( even when web-application is secured)
 --> url rewriting is stored in another location aswell: 
     3rd party content leads to another website (e.g: amazon.de) when the user clicks it the browser will automatically send the URL that referred him (referrer-header)
	 This url is now stored on a webserver log of a 3rd party, over which you have no control.
 --> URL Rewriting is not recommended for session management!
 
 BASIC AUTHENTICATION:
 --> is build-in in the HTTP-Protocol to authenticate a user
 --> is just a configuration-option on the server often in .htaccess file
 --> could also be implemented in web-application (NOT RECOMMENDED)

 basic authentication does not create a separate session token, meaning username and password are sent with every request.
 if Basic Authentication is needed, use TLS an unencrypted web-port is disabled. 
 (nevertheless an attacker could redirect you to a site that does not need TLS through DNS spoofing.
 
 
 COOKIES:
 We can control which servers and which application paths a cookie will be sent to.
 That can be done via the "DOMAIN" and "PATH" attributes of the cookie. (check your API how to do that)
 
 "DOMAIN"-Attribute lets you specify whether a specific host or a domain of hosts that you which the cookie to be sent to.
 if the Domain-attribute is set to  a fully qualified name with no leading period, then the cookie will be sent to only that specific host.
 e.g.: www.example.org

 if the value of the domain attribute begins with a period ".example.org" then the cookie will be sent to every host
 whose fully qualified name ends with example.org.
 The path attribute allows you to refine it further:
 Store.example.org
 
 
 Expiration date:
 if it is set to future date, the cookie is stored and can be reused for other sessions
 when cookie-date is in past or not exist at all, the cookie is used only for this session, and never been written to disc cache.
 
 "SECURE"-attribute ?? what is it called
  --> allows marking the cookie secure
  --> and TLS is required
  --> if unencrypted request, the cookie will not be sent
  

Session-cloning is an attack that uses Session-management.
The real risk of failing to protect session tokens is best termed SESSION-CLONING
Some of these attack tools are references to cloning. "SHEEP", "FIRESHEEP", "j-BAAH"

*Session fixation attack*
Whenever our application changes state the old session ID should be expired and a new session ID created.
We will define a change in state to be changing our encryption status or changing authentication status.
if we do not change the session id, nothing prevents the attacker 
from visiting the site to obtain a valid session ID and then including that session ID in a phishing email to users. 
If a user clicks through and eventually authenticates to the site, the attacker now has a valid session ID 
as an authorized user even though he may not know exactly who that user is.

The session-id MUST BE REPLACED after authentication.
The session-id must be of sufficient size (not id=221) and random, not generated from the user ID.

*Implement Session tampering measures:*
Session tampering is when a user manually attempts to modify his session token or an attacker who is attempting to brute force the session token.
A well designed application should be able to detect attacks such as these.
 
Example: 
*) For example can our application detect that a single host has attempted to
use more than one session ID in a given period of time?
*) Or can our application detect that a single session ID is in use from more than one address?
 
A common approach to this is to associate the session-id not only to the user 
but his IP address when he authenticated. 
 
While this is a useful approach, there are a few things that you should be aware of.
1)if multiple users are coming from behind the same proxy server, 
they will all have the same IP address. This makes it impossible to detect 
tampering by IP address for those users.

Also realize that you may find that in some parts of the worldh, large populations of people 
will appear to come from just a handful of IP addresses.
 Also IP-Adress of users can change (mobile-users,...)
 
Another option you might help more helpful:
Create a unique hash identifier of each browser and track the hash. 
Frequently you will find language support, plugins, add-ons and other items listed in the User Agent field
which can be used to provide a fingerprint for that browser.

Finally you can add tampering prevention field within the pages themselves
that is unique to each page and each user. If the correct value is not sent in in every request,
the session is automatically expired.
Problem: issues with back button
 
 
 
 
## XSS- Cross Site Scripting

 
*) XSS vulnerabilities occur when "evil" data is rendered by a web application.
*) This "evil" data or code allows an attacker to perform unintended and malicious actions
*) make sure to contextual encode untrusted data.
*) Causes untrusted code to be rendered and executed on a client browser.

How does this "evil" data make it into your app?
XSS code can be injected anywhere that **untrusted data** is used as input to a web site. 
Untrusted data is information that is **not hard-coded** into the application. 
Some common sources of untrusted data include web services, databases, and yes, our own end users.
 
XSS into three different flavors that help describe how the "evil" data is injected subsequently used by your app:
*) Stored or persistent XSS
*) Reflected XSS 
*) DOM based XSS
 
The only difference between these three variations is how the data is stored and processed by the application. 
But, the key thing, and this is really important to remember, is that XSS
occurs at the point where your app processes this "evil" data in the browser.

*) Stored XSS occurs when some "evil", untrusted data is saved by your app and later executed in the browser.
*) Reflected Cross-Site Scripting occurs when untrusted query parameters are sent to the application on the server and then echoed
back in the browser.
*) DOM-based XSS occurs when untrusted data is processed by JavaScript code used by your app.

How does it work? (Focusing on stored XSS)
javascript code is stored together with input-data therefore it is the possible to:
* Log keystrokes
• Conduct intranet port scanning,...

Tools do that for you BeEF (Browser Exploitation Framwork) 
you can create XSS on your own.

HOW TO PREVENT XSS?
*) ENCODE all untrusted input (transforming data into another form)
	In the case of XSS it means transforming potentially evil data into a benign, or non-executable, form.
	aus "<" wird "&lt;"
*) choose the appropriate transformation to change specific characters into a benign form. (CSS, JS
   at www.owasp.org there is a XSS prevention cheat-sheet.
*) INPUT VALIDATION on any untrusted data  (any data not hardcoded)
   includes data from webservices, data-bases and users 
*) make sure data contains acceptable letters only
*) but contextual encoding is best way to prevent xss
*) can be quite tricky as there are a lot of places in the app where XSS can occur:
   can be very difficult for devs to identify and fix all these instances

That's why many web application frameworks do automatic contextual
encoding, e.g.: 
WebProtection Library (.net)
Java ESAPI Struts, JSTL


## 04. Insecure Direct Object References

*) Ammounts to an failure to properly verify that the user is authorized to view any particular data.
*) MEANS: web application relies on information provided by the user to determine
which records within the database to access and display.
*) very specifiy situation, e.g:  
imagine a banking application. After successfully logging in the user is lists his account + account number
what happens when the user changes the account-number(in the url). The user could
potentially begin viewing someone else's records.
This is where the vulnerability can be exposed.
The example with account number is obvious.. However, this type of flaw becomes extremely
common when the application is referencing information from many different tables.

There are generally 2 approaches to resolving an insecure direct object references issue.
1) ALWAYS verify the user's authorization to view any piece information upon every request.
when the user requests information, the application would first verify that the user is authorized to see that information.

2)indirect object references. An indirect object reference is simply a value used within that particular
session or application to represent the actual value of the key within the database table.
e.g.: don't use ?employee=1, ?employee=2, ?employee=3,... but user ?employee=XSMD, ?employee=EZUD, ?employee=AMDA
and in the table do the mapping between XSMD <--> 3   (Security through Obscurity)

An insecure direct object reference vulnerability has been discovered in a URL parameter in your web application. 
Which of the following solutions would address this issue?
A: Store the identifier in a session variable



## 05. Security Misconfiguration

*) Outside of developers control
*) general problem is INSECURE CONFIGURATION
*) the application is only as secure as the foundation which it is built. 
E.g.: Web applications are sometimes deployed with default accounts or test accounts enabled. 

FIX IT BY:
*) Ensure that your development and test environments always mirror the production environment as closely as possible, allowing
for thorough testing of not only the application, but also config-settigns + patches,..
*) Verify that all default and test accounts have been removed or disabled in the production system
*) Ensure that any administrative interfaces that are not absolutely essential have been disabled, and not accessible from internet
*) Ensure that the configuration of the web service itself is appropriate, not permitting access to portions of the server that are
not necessary and that the file access permissions
*) Verify that the configuration prevents unexpected errors from revealing too much
Solution:
*) Unnecessary plugins are disabled or uninstalled.


***Example 1:***
The first example is the basic configuration of the server itself. 
As a developer you will want to ensure that the configuration of the
server that you use in development and that testers use in the quality assurance process
match the configuration of the actual production server as closely as possible. This will allow
you to easily verify whether or not the server operating system, web service and any frameworks
or libraries that your application relies on are kept completely up to date.

***Example 2:***
Another example of a secure configuration
problem would be the default installation of an administration interface. 
E.g.: every major java application server to serve web applications (Tomcat, WebLogic, WebSphere, JBoss, Jonas and more)
has an interface built in. This administrative interface is often running at a high numbered port separate from your application.
though it is sometimes accessible through a specific URL on the normal web ports. 
Just a matter of time until attackers will find it, and launch a password attack.

***Example3:***
Weak permissions  or open settings in the web service itself. (-rwxrwxr-x)
Are users allowed to view or access a directory where configuration or other sensitive information is stored.
Simple google searches reveal thousands of web applications where improper configurations will allow you to 
download user account database files, configuration files for the sequel server connection,...

***Example4:***
In java servers when there is an error, the stack trace will be shown.
This gives the attacker a different perspective on the application because he can now see more
of the underlying structure of the application and the frameworks that it is built in.


06. Sensitive Data Exposure A
*) Insecure Cryptographic Storage means that our application is storing sensitive information without proper cryptographic controls.
*) As a result of insecure cryptographic storage or insufficient transport layer protections, sensitive data is exposed or compromised.

How can you be sure that a database administrator can not view data he is not supposed to do?
The answer is encryption.

Storage of credential information
*) hash the password with strong algorithm (but not enough)
*) but hash will be the same for the same password (if 50 users choose "hello" as their password, the hash will look the same for them
   if user compromises 1 password he has access to all 50 users.
*) Solution to this is "salt". a random value that is prepended on the cleartext password, and then the hash is generated.    

Symmetric encryption: simply means that when the data is decrypted, the key that was used to encrypt the data
is used. In other words, the key is "symmetric" or lets say hidentical.

Example (Symmetric key): 
Company stores the user's credit card number.
When the company receives the number it is passed through an encryption function using the key and then stored in database.
When an attacker compromises the system, the key is most likely also compromised. therefore he can decrypt the credit-card number.

Asymmetric encryption/ Public Key Cryptography
*) here the key for encrypting and decrypting is not the same.
*) now when the company gets the credit-card number a random symmetric key is generated, to encrypt the credit card number.
*) now the Random symmetric key is encrypted itself with the public key from the asymmetric key pair. Finally the encrypted data
and the encrypted key are stored. ( a bit more efford but tremendous benefits

Advantages:
*) our periodic key change does not require that we decrypt all of the sensitive data.
   Only the individual symmetric keys need to be decrypted with the old private key and then re-encrypted with the new public
key.
*) the key for decrypting the data, the private key, need not be located in the path that is accessible to the public in general.
   That means even stealing a complete copy of the database,the sensitive data is irretrievable since the private key is simply not available. 

Note:
Mind the logging output. There might be not encrypted data through testing,...
Be sure before you migrate to prod-environment that any additional logging is either
disabled or stored in a secured, encrypted  location.
   
IN DB: 
USER HASH SALT

## 06. Sensitive Data Exposure B

Sensitive Data Exposure that can be addressed through properly encrypting data while in transit.

If you begin to encrypt, you should encrypt everything 
Session-token needs to be replaced when there is a change of state.

Change of state = (e.g.: authenticated to unauthenticated)

Mixed frames / mixed elements
When an images contains an absolute link to a non TLS path, but the rest of the page is encrypted.
Encrypt everything! Every element needs 

SECURE COOKIE
User is on a secure page and application issues an session cookie. browser sents that cookie in every request.
if an attacker can bring a user to a non TLS page, he could read the sensitive cookie-data (in clear text).
Thats why there is a secure cookie.
Means the cookie will only be sent if there is a secure connection.h

Correct implementation
of an TLS secured site requires that the certificate installed
on the server matches both the name and the adress of the server and the certificate must be current
(mybank.example.com 192.168.1.1 mm:dd:yyyy) 

What should be expired and reissued when the application begins encrypting data?
-->Session tokens.

How can we prevent cookie-based session tokens from being sent with unencrypted HTTP requests?
-->Use the “Secure” cookie setting.


## 07. Missing Function Level Access Control

Means that an attacker can access resources on our server or in our application directly,
bypassing the normal application flow and, possibly, bypassing any security controls
that we have in place.

Forced browsing:
The attacker is forcibly changing the URL within his browser, completely ignoring the normal sequence of events within the web

level is to require mandatory access controlsfor all resources within the application.
that all access to the information is completely mediated, or that there will be something standing between the user and the information.
There must be a single path through which all access is mediated or processed.

Mandatory Access Controls:
Mandatory access controls simply means that granting a user access to the system does not necessarily grant them access to
any information. Instead, all access must be specifically granted, usually through the
implementation of roles or rights.

Example1:
Mandatory Access Control + Complete Mediation could look like that:
application where all of the requests for information are tunnelled through a single piece of code. 
Some refer to this as a Gatekeeper. The gatekeeper knows to first verify that the user is authenticated. 
Next, whenever the user requests some resource, the gatekeeper first consults the mandatory access control
lists that apply to the resources being requested.
Since every resource has a defined access control list, usually defined in terms of roles or rights, if the user account does
not have the proper rights, the gatekeeper will refuse access, never even loading the resources in question.
With this type of system even forced browsing and data tampering will fail since the gatekeeper
will never allow an unauthorized or unauthenticated user to see or access sensitive resources.

Example2:
Another common failure in implementing function level access control is in the configuration of the server or application. For instance,
perhaps there is an administration panel for the web application or the server itself.
Normally, only the administrator would use this interface. There may not even be a link
to this interface anywhere on the web pages. Even so, nothing would stop an attacker from simply 
modifying the URL to point to the administration panel through forced browsing.
 Rather than rely on the secrecy of the administration URL, it is far better practice to either disable
access to the administration panel through Internet facing interfaces or, if it is necessary
for the administration panel to be Internet facing, to apply the same mandatory access
control system to the administration panel.

IP-Adress restriction may help



##  08. Cross Site Request Forgery (CSRF/XSRF)
takes advantage of the fact that it is not actually the user who is authenticated to a web application, it is actually the web
browser that is authenticated. Since web communication is stateless, web applications must create a session during he authentication process. In order to track
the user's activities throughout this session,a session token is created. This token is
some unique piece of information to identify that particular instance of the user and is
sent to the browser and cached or possibly embedded into every URL on the page. In every
subsequent request that follows, the session token is then sent back to the web application
and used as the basis for most authorization activities.
A side effect of this model of session management is it is trivial for an attacker to convince the browser to send an arbitrary request to
the website on behalf of the user without the user necessarily being aware of the activity.
In many cases this is not a serious issue.
 
Attacks that leverage cross site request forgery can be accomplished in a number of ways. It
might be that the web application accepts all of its form parameters through the use 
of a get request, which is not good practice, and there are no additional defensive mechanisms
in place. Should this be the case, it becomes a trivial matter to include an image tag,
eye frame, or other clickable link that will either automatically, or possibly with user interaction, submit the transaction request
along with the session information to the vulnerable site. As long as the user has a
current and valid session token, the attack will succeed.

CSRF attacks:
May occur without the user ever realizing it, as a result of hidden frames or image tags.
CSRF takes advantage of the fact that:


Prevent XSRF:
In clude an unpredictable token of some kind in the body, the URL or
the headers that are sent between the client and server.
In some implementations this additional token might be unique to that particular session,
while in other implementations this token is unique to the individual request being made.

***Whats the difference to the session-token?***
The primary difference is that the session token is typically placed in a location that will cause it to be automatically
sent in every request. For example, within a cookie. However, the cross site request
forgery protection token should not be placed in a cookie. Instead, it would typically appear
somewhere within the body of the page, perhaps as a hidden form field or even within the
submitted URL. Should you choose to place the token within the URL, be aware that this
does create a potential exposure point where that token could be intercepted by the attacker.


Another way to protect against XSRF
by requiring the user to re-authenticate with his actual credentials whenever any substantial change will happen within the application.

OWASP CSRF guard Project contains  a library for PHP, JAVA, .NET. 
ESAPI from OWASP contains token generators 

## 9. Using Known Vulnerable Components
--> Components need to be uptodate.
--> not using 3rd party components is also not practical 

How to maintain the application:
*) When selecting a third party plugin it is best to, select only the most up-to-date and best maintained option.
*) consider creating and maintaining a master list of all third party components and versions that
*) Whenever your code references or makes use of a third party component, broker the requests through this wrapper framework.
*) A good approach toward securing third party components is to create a standardized wrapper framework to broker and control all data and access to components.
*) which version is vulnerable?


## 10. Unvalidated Redirects and Forwards

*) takes advantages the users trust in a website
*) user is sent to a 
*) captive portals (e.g.: login to WIFI)

In conclusion, including code that will redirect
a user to an external resource can be abused by attackers


HOW TO DEAL WITH IT:
*) avoid having a redirect function within our application. 
*) if is not an option consider using some sort of reference or mapping value in the redirect rather than using a raw URL
*) This mapping value, likely an id for an item in a database, can be used on the server side to determine which URL to redirect the user
to. In this way, all of the redirects will be authorized since we are never relying on what the user passes in to generate the redirect message itself.
*) If our application implements a redirect function it is important to, ensure that the function will only redirect users to specific URLs.












