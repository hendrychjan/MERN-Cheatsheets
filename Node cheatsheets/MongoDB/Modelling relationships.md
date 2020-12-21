# MongoDB modelling relationships

- three approaches: consistcy X query performance X hybrid

## Using references (Normalization)

- more constistent

```js
let author = {
  name: "John",
};

let course = {
  author: "id",
};
```

## Using embedded documents (Denormalization)

- higher query performance

```js
let course = {
  author: {
    name: "John",
  },
};
```

## Hybrid

```js
let author = {
    id: "15asd8dw86awd46",
    name: "John"
    // many other properties
};

let course = {
    name: "Programming course,
    author: {
        id: "15asd8dw86awd46", // reference
        name: "John" // often used properties
    }
};
```
