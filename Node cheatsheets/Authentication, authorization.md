# Authentication

## Dependencies

- `mongoose`
- `joi`
- `jsonwebtoken`
- `config`
- `lodash`
- `bcrypt`

### User model

- first of all, you need to creat a `user` model:

```js
// Import dependencies
const mongoose = require("mongoose");
const Joi = require("joi");
const jwt = require("jsonwebtoken");
const config = require("config");

// User model for MongoDB
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String,
});

// Method for jwt token generation
// -> note that here I am loading "jwtKey" from ENV - to learn about this, there is a "Storing sercets.md" document within repo
userSchema.methods.generateAuthToken = function () {
  const token = jwt.sign({ _id: this._id }, config.get("jwtKey"));
  return token;
};

// Joi validation (for instance when creating a new user)
function validateUser(user) {
  const schema = Joi.object({
    name: Joi.string().min(1).max(25).required(),
    email: Joi.string().min(5).max(50).email().required(),
    password: Joi.string().min(6).max(16).required(),
  });

  return schema.validate(user);
}

// Wrap userSchema and generateAuthToken() onto mongoose model
const User = mongoose.model("User", userSchema);

// ...And export the whole module
module.exports.User = User;
module.exports.validateUser = validateUser;
```

### Auth middleware

- now, you need to create a middleware that will take care of authenticating user by provided token
- for this, create a new file `/middleware/auth.js`:

```js
const config = require("config");
const jwt = require("jsonwebtoken");

// Note that function is immidiatelly exported
module.exports = function auth(req, res, next) {
  // Read token provided in request header
  const token = req.header("x-auth-token");
  // If it isnt provided (=> doesnt exist), denie access
  if (!token) return res.status(401).send("Access denied. No token provided.");

  // If the token is provided, verify it
  try {
    const decoded = jwt.verify(token, config.get("jwtKey")); // Again, reading "jwtKey" from ENV
    req.user = decoded;
    next(); // Token was successfuly verified - grant access and continue to http route handler
  } catch (ex) {
    // Token verification failed - token is invalid
    res.status(400).send("Access denied. Invalid token.");
  }
};
```

### Auth http route

- create `/routes/auth.js` that will take care of authenticating users (loging in)

```js
// Import dependencies
const _ = require("lodash");
const express = require("express");
const bcrypt = require("bcrypt");
const Joi = require("joi");
const moglib = require("../custom_modules/moglib");
const { User } = require("../models/user");

// Define http router
const router = express.Router();

// This route will validate given credentials and authenticate user
router.post("/", async (req, res) => {
  // Check if supplied credentials are valid
  const { error } = validate(req.body);
  if (error) return res.status(400).send(error.detail[0].message);

  // Attempt to find user under given email (in MongoDB)
  let user = await User.findOne({ email: req.body.email });
  if (!user) return res.status(400).send("Invalid password or email."); // user under given email was not found

  // If user was successfuly found, check his password
  let validPassword = await bcrypt.compare(req.body.password, user.password);
  if (!validPassword) return res.status(400).send("Invalid password or email."); // given password is invalid

  // User was found, password is correct => user is authenticated => generate and return token
  let token = user.generateAuthToken();

  res.status(200).send(token);
});

// Validate supplied credentials (what credentials does the user have to supply? -> email and password)
function validate(req) {
  const schema = Joi.object({
    email: Joi.string().min(5).max(255).required(),
    password: Joi.string().min(6).max(255).required(),
  });

  return schema.validate(req);
}

module.exports = router;
```

- note that we never specify whether email or password is incorrect -> that would make things easier for hackers

### Registrate auth http route

- now, in `index.js` registrate the auth route (endpoint):

```js
// Import module
const auth = require('./routes/auth');

// Registrate auth as a route
app.use("/api/auth", auth);
```

### Use auth middleware
- to use auth middleware, just suply it as parameter in route function
- example: (only authenticated users can perform delete somewhere)
```js
router.delete("/:id", auth, async (req, res) => {
    // find by id and delete...
});
```

### Manage users

- finally, you need a http route to manage users
- so, create `/routes/users.js`:
```js
// Import dependencies
const _ = require("lodash");
const auth = require("../middleware/auth");
const express = require("express");
const bcrypt = require("bcrypt");
const { User, validateUser } = require("../models/user");

const router = express.Router();

// This route (if valid token is provided) returns user's profile
router.get("/me", auth, async (req, res) => {
    // Get user info except password (it is hashed anyways)
    let user = await User.findById(req.user._id).select('-password');
    res.status(200).send(user);
});

// Create new user
router.post("/", async (req, res) => {
    // Validate user credentials
    const {error} = validateUser(req.body);
    if (error) return res.status(400).send(error.details[0].message);

    // Check - mby user already exists?
    let user = await User.findOne({email: req.body.email});
    if (user) return res.status(400).send("This email is already being used.");

    // If user seems to be unique, create it
    user = new User(req.body);
    const salt = await bcrypt.genSalt(10); // Generate salt for hash algorithm
    user.password = await bcrypt.hash(user.password, salt); // Hash password
    user = await user.save(); // Save user to database

    // Now, as user is created, give hin auth token
    let token = user.generateAuthToken();
    user = _.pick(user, ["_id", "name", "email"]); // Exclude password from response
    res.status(201).header("x-auth-token", token).send(user); // put token to response header
});

module.exports = router;
```

### Registrate users route
- last thing is to registrate users route in `index.js`:
```js
const users = require("./routes/users"); 
app.use("/api/users", users);
```