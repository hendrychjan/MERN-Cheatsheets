# Express routing
### Routing structure
- in root of Node application, create folder "routes"
- in this folder, js files handling specific routes will be located
- create route like so:
```js
// In this case, we are creating router for handling CRUD operations around "notes"
// Import express modules
const { Router } = require('express');
const express = require('express');

// Define router
const router = express.Router();

// Perform HTTP service using router

// URL already contains "api/notes"
router.get('/', (req, res) => {
    // Handle GET
});

router.put('/:id', (req, res) => {
    // Handle put
});
// ... and so

// export module
module.exports = router;
```

### Use router in index.js
- now, use router module in index.js
```js
const notes = require('./routes/notes'); // Import module
app.use('/api/notes', notes); // Use notes router on this URL