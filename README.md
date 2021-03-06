![alt text](https://raw.githubusercontent.com/dman777/icons/master/express.jpg)        ![alt text](https://raw.githubusercontent.com/dman777/icons/master/node.jpg)        ![alt text](https://raw.githubusercontent.com/dman777/icons/master/npm.jpg)
##express-multi-proxy-router

#####Multiple router based on URL(s) for Express.js
#####Version 2.0.x Stable
######Todo: XML Support for body/payload. 

#####Whats new for Version 2.0:
Better error handling:
* Fixed issue when error could not be returned to client browser because of 
  circle dependency in json string returned
* Errors are now reported to console when error entails a http request that is local 
  to the server and can not be returned to client browser.

Enhancements: 
* http/https now supported. Previously, only https.
* Dynamic URLs will work. Previously, only static urls. 

**Typical scenerio:**

Suppose you are developing a web application that makes API requests from the client browser. Most likely, your browser will not allow you to because you will be breaking the pesky 'Same-origin policy'. To over come this, your express.js web server must send out http request and deliever the response back to the web client. Thus, this is known as proxying the http request(your api call out) from your `browser client -> to the express.js web server -> to the new host`. This module will allow you to do this. In addition, you can use as many proxies/routes as you would like.

**Http verbs Supported:**
patch, post, put, del, and head.

**Requirements(dependencies installed automatically)**:
* Express.js 4.0 or greater. Note- the differenceof 4.x is body-parser is no longer built in, but a required module of express.js.
* body-parser
* bluebird promises
* restler
* ini

---

####Note: place `process.env['NODE_TLS_REJECT_UNAUTHORIZED'] = '0';` into your express.js's `app.js` or similar express config file to turn off ssl cert checking.

**Installation instuctions:**

*Assumeing you already have a express.js or some kind of generator project already established:*

1. Go to the directory of your project, where you see `package.json` and `npm_modules`.
2. Simply type `npm install express-multi-proxy-router`
3. In your express.js configuration file(typically called `app.js`), place the middle ware as such in the last 2 lines where you a list of app.use().
```
app.use(bodyParser());
app.use(proxyUrl());
``` 
4.At the top of that same express.js configuration file(typically called `app.js`), place these 2 lines:
```
var bodyParser = require("body-parser");
var proxyUrl = require("express-multi-proxy-router"); 
```
---
**Usage and Configuration:**

Let's say in your client browser code you call out to /api/v1/goto-identity-api and /api/v2/get-list-of-servers. Both of these api's go to compeletley seperate hosts. You will modify your `proxies_config.ini` file located at `node_modules/express-multi-proxy-router/proxies_config.ini` with the following config:

```ini
[proxies]
#Add your hosts here in the format of 
#host = url

https://identity.api.foobar.com/v2.0/tokens = /api/v1/goto-identity-api
http://hosting.company.servers/api/servers = /api/v2/get-list-of-servers
```
*notice that that `https` or `http` is specified. Be sure you specify this when setting yours.*

After this is set, when the client browser sends out a http request to `/api/v1/goto-identity-api`, express.js will see that the url is `/api/v1/goto-identity-api`. It will then send out it's own request to `https://identity.api.foobar.com/v2.0/tokens` and return the response back to the client browser. 

There is no limit to how many proxies you can add, add as many as you like.


