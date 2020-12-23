# CRUD Validation
## Built-in validation
- validators:
    - required ```true/false/function```
    - minlength ```<number>```
    - maxlength ```<number>(```
    - match ```/pattern/```
    - enum ```[<array>]```
    - validate ```{validator: function}```
- use function as validator:

### Use of regular validators
```js
// Define validation in mongoose schema
const noteSchema = new mongoose.Schema({
    name: {type: String, required: true},
});

// Create model
const Note = mongoose.model('Note', noteSchema);

// Use validation on CRUD operations
async function createNote(_name) {
    // Define new object
    let note = new Note({
        name: _name
    });

    // Validate
    try {
        await note.validate();
    } catch (err) {
        // note is not valid - handle here
    }

}
```

#### required: function
- if property is required only in some cases
```js
// Price is required only if the course is published
const courseSchema = new mongoose.Schema({
    isPublished: Boolean,
    price: {
        type: Number, 
        required: function() {
            return this.isPublished;
        }},
});
```

### Use of custom validator
- use "validate" and set it to 
```js
const noteSchema = new mongoose.Schema({
    tags: {
        type: String, 
        validate: {
            validator: function(v) {
                // if v has a value and its length is greater than 0
                return v && v.length > 0;

            },
            message: "A note should have at least one tag"
        }},
});
```

### Async validators
```js
const noteSchema = new mongoose.Schema({
    tags: {
        type: Array, 
        isAsync: true,
        validator: function(v, callback) {
            // Timeout simulates work
            setTimeout(() => {
                const result = v && v.length > 0;
                callback(result); // equivalent of return
            }, 2000);
        }},
});
```

## Handling validation errors
```js
try {
    note.validate();
} catch (ex) {
    let errs = [];
    // Iterate through all validation errors - messages
    for (field in ex.errors) 
        errs += ex.errors[field].message;

    // Send result
    res.status(422).send("Unprocessable entity: " + errs);
}
```