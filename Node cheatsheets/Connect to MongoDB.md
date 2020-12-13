# Connect to MongoDB

### Create connection
```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/playground')
 .then(() => console.log("Connected..."))
 .catch(err => console.error("Connection failed..."));
```
- optionaly, connection string can be stored in json document, just like so:
```json

```