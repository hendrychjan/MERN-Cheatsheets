# Mongoose overview

### Install dependencies
```npm i mongoose``` - required MongoDB package

### Create connection
```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/playground')
 .then(() => console.log("Connected..."))
 .catch(err => console.error("Connection failed..."));
```

### Define a schema and model
- to store objects in MongoDB, it is neccessary to define a Mongoose schema
- _id is automatically part of schema, so you don't have to add it manually
```js
const noteSchema = new mongoose.Schema({
    title: String,
    context: String,
    isHighPriority: { type: Boolean, default: false }
});
```
- supported data types are:
    - String
    - Number
    - Date
    - Buffer (for stroing binary data)
    - Boolean
    - ObjectID
    - Array
    - []
    - {}

- supported Mongoose model properties are:
    - type
    - *validators:*
        - required ```true/false/function```
        - minlength ```<number>```
        - maxlength ```<number>(```
        - match ```/pattern/```
        - enum ```[<array>]```
        - validate ```{validator: function}```
    - max ```<number>```
    - min ```<number>```
    - lowercase ```true/false```
    - isAsync ```true/{not_set}```
    - get ```get: v => Math.round(v);``` (custom getter)
    - set ```set: v => Math.round(v);``` (custom setter)


- mongoose schema acts like a blueprint for model
- to create a model out of a schema:
```js
const Note = mongoose.model('Note', noteShema);
```