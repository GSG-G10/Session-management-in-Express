## What are sessions?
* **A session is used to temporarily store the information on the server to be used across multiple pages of the website**. It is the total time used for an activity. The user session starts when he logs-in to a particular network application and ends when the user logs out from the application or shutdowns the system.
* When we work on an application over the internet, the webserver doesn't know the user because the HTTP protocol does not maintain the state. The information provided by the user on one page of the application (Let's say Home) will not be transferred to another page. To remove this limitation, sessions are used. Session gets started whenever a visitor first enters a website.
* The user information is stored in session variables, and these variables can store any type of value or data type of an Object.
* Session values are much secured as these are stored in binary form or encrypted form and can only be decrypted at the server. The session values are automatically removed when the user shutdowns the system or logout from the application. To store the values permanently, we need to store them in the database.
* Each session is unique for each user, and any number of sessions can be used in an application; there is no limitation to it.
* The user is identified with the help of **sessionID**, which is a unique number saved inside the server. It is saved as a cookie, **form field, or URL.**


## Key Differences between Session and Cookies
* A cookie is a bit of data stored by the browser and sent to the server with every request.
* A session is a collection of data stored on the server and associated with a given user (usually via a cookie containing an id code)
* A session can store as much data as a user want, whereas Cookies have a limited size of 4KB.

## Working of Session
1. In the first step, the client request to the server via GET or POST method.
2. The sessionID is created on the server, and it saves the sessionID into the database. It returns the sessionId with a cookie as a response to the client.
3. Cookie with sessionID stored on the browser is sent back to the server. The server matches this id with the saved sessionID and sends a response **HTTP200**
## What are the different ways of managing sessions in express?
The Session and cookies are used by different websites for storing user's data across different pages of the site. Both session and cookies are important as they keep track of the information provided by a visitor for different purposes. The main difference between both of them is that sessions are saved on the server-side, whereas cookies are saved on the user's browser or client-side. Apart from this, there are also various other differences between both.

![](https://i.imgur.com/pYHLAtG.png)

## Session management in NodeJs
> We can use express-session middleware to manage sessions in Nodejs. The session is stored in the express server itself. The default server-side session storage, MemoryStore, is purposely not designed for a production environment. It will leak memory under most conditions, does not scale past a single process, and is meant for debugging and developing. To manage multiple sessions for multiple users we have to create a global map and put each session object to it. Global variables in NodeJs are memory consuming and can prove to be terrible security holes in production level projects.
This can be solved by using external Session Store. We have to store every session in the store so that each one will belong to only a single user. One popular session store is built using the Redis.

#### Session Options
> express-session accepts these properties in the options object.
<!-- * **Name**: It defines the name of the cookie.
* **Value**: It defines the value of the cookie.
* **secure** - Ensures the browser only sends the cookie over HTTPS.
* **httpOnly** - Ensures the cookie is sent only over HTTP(S), not client JavaScript, helping to protect against cross-site scripting attacks.
* **domain** - indicates the domain of the cookie; use it to compare against the domain of the server in which the URL is being requested. If they match, then check the path attribute next.
* **path** - indicates the path of the cookie; use it to compare against the request path. If this and domain match, then send the cookie in the request.
* **expires** - use to set expiration date for persistent cookies.

 -->

## **Import all the Node.js libraries that we explained earlier**
ExpressJS - Sessions Exampele
```
var express = require('express');
var cookieParser = require('cookie-parser');
var session = require('express-session');

var app = express();

app.use(cookieParser());
app.use(session({secret: "Shh, its a secret!"}));

app.get('/', function(req, res){
   if(req.session.page_views){
      req.session.page_views++;
      res.send("You visited this page " + req.session.page_views + " times");
   } else {
      req.session.page_views = 1;
      res.send("Welcome to this page for the first time!");
   }
});
app.listen(3000);
```
>  What the above code does is, when a user visits the site, it creates a new session for the user and assigns them a cookie. Next time the user comes, the cookie is checked and the page_view session variable is updated accordingly.
> Now if you run the app and go to localhost:3000, the following output will be displayed.
![](https://i.imgur.com/e40qz1n.jpg)

> If you revisit the page, the page counter will increase. The page in the following screenshot was refreshed 42 times.
![](https://i.imgur.com/ujuHprC.jpg)

