# HOW TO USE PASSPORT.JS FOR A USER REGISTRATION / LOGIN SYSTEM
---------------------------------------------------------------

* Dependencies - 'npm install --save ' these dependencies :
	a. express
	b. mongoose
	c. passport
	d. passport-local
	e. passport-local-mongoose
	f. body-parser
	g. express-session
	h. ejs

* Set up the file structure and Express app
```
// DIRECTORIES & FILES
// -------------------

-- /models
---- user.js
-- app.js
-- /views
---- login.ejs
---- signup.ejs
---- 
```

* Set up '/models/user.js'
```
var mongoose = require('mongoose');
var passportLocalMongoose = require('passport-local-mongoose');

var UserSchema = new mongoose.Schema({
	username: String,
	password: String
})

// Add passportLocalMongoose packages to the schema
UserSchema.plugin(passportLocalMongoose)

module.exports = mongoose.model('User', UserSchema);
```

4. Go back to 'app.js' and plug this code in to use express-session
```
app.use(require('express-session')({
	secret: 'Put a unique Secret string here',
	resave: false,
	saveUninitialized: false
}));
app.use(passport.initialize());
app.use(passport.session());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

5. To handle a signup/register form's 'username' and 'password' and hash the password
enter these in a post route.
```
User.register(new User({ username: req.body.username }), req.body.password, function(err, user) {
	if (err) { 
		// handle error 
	} else {
		passport.authenticate('local')(req, res, function() {
			// redirect to whatever page
	})
	}
});
```




