# CRUD operations on MongoDB

## Create

`<object>.save();`

> Important: MongoDB by default assigns unique ID to every object that you create

## Retrieve

- available methods:

```
find();
findOne();
findById();
```

- example use: `<mongoose_model>.findById("456asd8w5dw6864d684d6")`
  > to retrieve all records, run: `<mongoose_model>.find()`

### Filtering

- perform filtering by adding object as a parameter to the find() method, just like so:

```js
Course.find({ author: "Joe Rubbens", isForFree: false, price: 10 });
```

#### Comparison query operators

- list of comparison operators:
  - `eq` => equal
  - `ne` => not equal
  - `gt` => greater than
  - `gte` => greater than or equal to
  - `lt` => less than
  - `lte` => less than or equal to
  - `in` => in (range)
  - `nin` => not in (range)
- examplary use:

```js
// Select where price is more or equal to 10 AND less then 20
.find({ price: { $gte: 10, $lt: 20 } });

// Select where price is either 10 or 20 or 30
.find({ price: { $in: [10, 20, 30] } });
```

#### Ligical query operators

- list of logical operators:
  - `or`
  - `and`
- examplary use:

```js
// Select where author is Joe Rubbens OR the price is more than 10
.find().or([ { author: "Joe Rubbens" }, { price: { $gt: 10 } }]);

// Select where author is Joe Rubbens AND the price is 10
.find().and([ { autor: "Joe Rubbens" }, { price: 10 } ]);
```

#### Regular expressions

- syntax:

```js
.find({ key: /pattern/ });
```

- exemplary use:

```js
// To select "June", "July" -> those which start with "Ju" (case insensitive)
.find({ month: /^Ju/i });
```

- to learn more about reglar expressions in JavaScript, follow [this link](https://www.w3schools.com/jsref/jsref_obj_regexp.asp)

### Sorting, counting

```js
.limit(5); // Limits the number of results to 5
.sort({ price: 1 }); // Sort by price in ascending order
.select({ name: 1 }); // Select only names
.count(); // Returns number of results
```

- when sorting, 1 indicates ascending order - in case you need to use descending order, use 0

#### Implement pagination

Example:

```js
let pageNumber = 2;
let pageSize = 10;

let notes = await Note.find({ isImportant: true })
  .skip((pageNumber - 1) * pageSize)
  .limit(pageSize)
  .sort({ name: 1 });
```

## Update
- find > modify properties > save > return updated
```js
async function updateNote(id) {
    let note = await Note.findById(id);
    if (!note) return; // Return if note was not found

    // Option 1:
    note.isImportant = true;
    note.name = "Programming ideas";

    // Option 2: --this option is obsolete - might not work for you
    note.set({
        isImportant = true,
        name: "Programming ideas"
    });

    let result = await note.save();
    return result; // Return updated note
}
```

## Remove
- remove specific object
```js
async function removeNote(id) {
    let res = await Note.deleteOne({ _id: id}); // Delete note
    return res; // Return deleted note
}
```

- remove objects by some key
```js
async function removeClosedNotes() {
    // Delete all notes that are closed
    let res = await Note.deleteMany({ isClosed: true });
    
    return res; // Return deleted notes
}