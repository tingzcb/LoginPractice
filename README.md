# Overview
This project is from `Youtube` study, and the video link is [ Node.js Passport Login System Tutorial](https://www.youtube.com/watch?v=-RCnNyD0L-s&t=549s 'Web Dev Simplified')

# Run the project
```
npm run devStart
```

Try to learn fast and go through


# Learn Points
## How to use `Github`
## get & post method used to post form
post method can appends the form data in the body of HTTP request, and it is no size limitation and will not show in browser address.
## understand passport authentication middleware
## `serializeUser`


# Steps


create login.ejs, register.ejs, files
then add router, use get route

form post method, post the form into '/ register', instead of our server

```
app.use(express.urlencoded({extended : false}))
// the code allow to access `req.body.name
```


```
npm i bcrypt
//allow to hash password
```


```
npm i method-override
// to implement app.delete('/logout')
//override our post request
```
# Passport.js 
[passport.js authentication document](https://www.passportjs.org/concepts/authentication/downloads/html/)

to help different users to access match pages
```
npm i passport passport-local
```

```
npm i express-session express-flash
```



# Need to Deep Study

`serializeUser`


```
users.find( user => user.email === email)
```

flash 

session 


## passport

Study Document Website [npm passport](https://www.npmjs.com/package/passport)
### Strategies
Before authenticating requests, the strategy (or strategies) used by an application must be configured.
```js
passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) { return done(null, false); }
      if (!user.verifyPassword(password)) { return done(null, false); }
      return done(null, user);
    });
  }
));
```


## Sessions
Passport will maintain persistent login sessions. In order for persistent sessions to work, the authenticated user must be serialized to the session, and deserialized when subsequent requests are made.

Passport does not impose any restrictions on how your user records are stored. Instead, ***you provide functions to Passport which implements the necessary serialization and deserialization logic.*** In a typical application, this will be as simple as serializing the user ID, and finding the user by ID when deserializing.

```js
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id, function (err, user) {
    done(err, user);
  });
});
```

How to learn Section 
-------------------
1. read the document
2. combine few parts, following the tutorial
3. read the code again
Do not worry!!!  


## Middleware
To use Passport in an [Express](http://expressjs.com/) or [Connect](http://senchalabs.github.com/connect/)-based application, configure it with the required `passport.initialize()` middleware. If your application uses persistent login sessions (recommended, but not required), `passport.session()` middleware must also be used.

```js
var app = express();
app.use(require('serve-static')(__dirname + '/../../public'));
app.use(require('cookie-parser')());
app.use(require('body-parser').urlencoded({ extended: true }));
app.use(require('express-session')({ secret: 'keyboard cat', resave: true, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());
```

## Authenticate Requests

Passport provides an `authenticate()` function, which is used as route middleware to authenticate requests.
```js
app.post('/login', 
  passport.authenticate('local', { failureRedirect: '/login' }),
  function(req, res) {
    res.redirect('/');
  });
```


#  Explain Note for  code in point


```js
  if(user == null){

            return done(null, false, { message: ' No user with that Email'})

        }
       //null return error on server, now there is not error on server
       //false  return the user that we found, now no user, so return false
       // return a message, that will be displayed
```

```js
initializePassport(

    passport,

    email =>{

        return users.find( user => user.email == email)
        //find the user base on email

    }

    ) // calll the function with passport parameter
```

```js
//load in environment variables and set them in process.env
//in development 
if(process.env.NODE_ENV !==  'production' ){

    require('dotenv').config()

}
```

```js
app.use(session({

    secret: process.env.SESSION_SECRET,

    resave: false 
    //stop resave if nothing change
    saveUninitialized:false
    //save emplty value in the session

}))
```

#important
```js
//check user authenticated to access the main page function 
//checkAuthenticated middleware function 
function checkAuthenticated(req, res, next){

    if(req.isAuthenticated()){

        return next()

    }
	// if the req authenticate is false, redirect to the login page
    res.redirect('/login')

}
```

# troubleshooting/Error that met during practice
```javascript
Error: req#logout requires a callback function
```
[https://stackoverflow.com/questions/72336177/error-reqlogout-requires-a-callback-function](https://stackoverflow.com/questions/72336177/error-reqlogout-requires-a-callback-function)

The other major change is that that `req.logout()` is now an asynchronous function, whereas previously it was synchronous. For instance, a logout route that was previously:

```javascript
app.post('/logout', function(req, res, next) {
  req.logout();
  res.redirect('/');
});
```

should be modified to:

```javascript
app.post('/logout', function(req, res, next) {
  req.logout(function(err) {
    if (err) { return next(err); }
    res.redirect('/');
  });
});
```

