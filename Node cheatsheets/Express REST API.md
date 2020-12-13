# RESUful API with Express

### REST Conventions

- REST defines a set of conventions for creating HTTP services:
  - POST: create resource
  - GET: read resource
  - PUT: update resource
  - DELETE: delete resource

### NPM dependencies

- To work with express, install these packages:
  `express` - required; it is express itself
  `nodemon` - optional - it watches for changes and automatically
  `morgan` - optional - middlewaren logs
  restarts node server, so you don't have to do it yourself

### Build web server

```js
const express = require("express");
const morgan = require("morgan"); // OPTIONAL
var port = process.env.PORT || 3000; // read port from env var || 3000

const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(morgan('tiny')); // OPTIONAL Morgan middlewaress

// Routes or router go here

app.listen(port, () => console.log("Listeting on " + port));
```

### Defining routes

- it is recommended, to separate code for routes from index.js
- to achive such thing, use Express routing

> examples below use "notes" as subject/resource

#### POST - Create

```js
app.post("/api/notes", (req, res) => {
  let note;
  // Create new note and return it as object
  res.send(note);
});
```

#### GET - Get all

```js
app.get("api/notes", (req, res) => {
  let notes = []; // Load notes from database

  // OPTIONAL (sorting):
  // To read query string parameters (?sortBy=name)
  let sortBy = req.query.sortBy;

  res.send(notes);
});
```

#### GET - Get specific

```js
app.get("api/notes/:id", (req, res) => {
  let noteID = req.params.id; // Read note id from request URL

  let note; // Lookup the note in database

  if (note) res.send(note);
  // Note was successfuly found
  else res.status(404).send("Note under given ID was not found.");
  // ^ Note was not found
});
```

#### PUT - Update

```js
app.put("api/notes/:id", (req, res) => {
  let noteID = req.params.id; // Read note id from request URL

  let note; // Lookup the note in database

  if (note)
    // Update note and return it
    res.send(note);
  // Note was successfuly updated
  else res.status(404).send("Note under given ID was not found.");
  // ^ Note was not found
});
```

#### DELETE - delete

```js
app.delete("api/notes/:id", (req, res) => {
  let noteID = req.params.id; // Read note id from request URL

  let note; // Lookup the note in database

  if (note)
    // Delete note from database and return it to API user
    res.send(note);
  // Note was successfuly updated
  else res.status(404).send("Note under given ID was not found.");
  // ^ Note was not found
});
```
