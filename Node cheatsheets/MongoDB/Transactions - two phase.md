# Two phase transactions
- we use transaction to create a stream of operations that we want to be executed on server
- if one of these operations fails, the stream is stopped and any other following operation will be executed

### Dependecies
```npm i fawn```

### Initialize fawn
- to use fawn, we first need to initialize it
- here is an example of initializing Fawn that will be used aside mongoose:
```js
const Fawn = require('fawn');
const mongoose = require('mongoose');

Fawn.init(mongoose);
```

### Example usage
- in this example, I create a new rental of a movie
- if such rental is successfuly created, its numberInStock is decremented
```js
try {
    new Fawn.Task()
      .save('rentals', rental)
      .update('movies', {_id: movie._id}, {
        $inc: { numberInStock: -1}
      })
      .run();

      return res.status(200).send(rental);
  } catch (e) {
    console.log(e);
    return res.status(500).send("Internal server error");
  }
```
> note: in real-world example, error would be logged like so: [how to log errors](ttps://medium.com/google-cloud/whats-the-best-way-to-log-errors-in-node-js-b3dfd2fe07a7)