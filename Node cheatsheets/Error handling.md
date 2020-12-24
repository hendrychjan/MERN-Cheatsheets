# Error handling

### Dependencies

- `express-async-errors`
- `winston`
- `winston-mongodb`

### Handling rejected promises

- whenever there occurs an error in promise, node server will stop
- to prevent this from happening, you need to wrap all async calls with try-catch block -> that is what `express-async-errors` package does for you
- now, in `/middleware/error.js` specify, how errors will be handled:

```js
// Import dependencies
const winston = require('winston');

module.exports = function (err, req, res, next) {
  winston.error(err.message, err); // log error using winston
  res.status(500).send("Internal server error.");
};
```

- now modify `index.js` as following example shows:

```js
// import dependencies
require("express-async-errors");
require("winston-mongodb");
const winston = require('winston');

// configure winston to log errors to "err.log" file
// and optionally to database 
// (replace 'mongodb://localhost/myproject)' with connection string to your database
winston.configure({
  transports: [
    new winston.transports.Console({ colorize: true, prettyPrint: true }),
    new winston.transports.File({ filename: "err.log" }),
    new winston.transports.MongoDB({db: 'mongodb://localhost/myproject'})
],
});

// Handle all uncaught exceptions that happen outside express
process.on("uncaughtException", (ex) => {
  winston.error(ex.message, ex);
  process.exit(1);
});

// Handle all unhandled rejections (promises) that happen outside express
process.on("unhandledRejection", (ex) => {
  winston.error(ex.message, ex);
  process.exit(1);
});

// import custom middleware error handler, that you created earlier
const error = require("./middleware/error");

// all middleware functions go here (routes, midlleware)
// ...app.use();...
// ...app.use();...
// ...app.use();...

app.use(error);
```
