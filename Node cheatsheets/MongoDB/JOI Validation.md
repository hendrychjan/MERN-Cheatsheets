# JOI Validation
### Dependencies
```npm i joi```

### Create model with validation
```js
// import dependencies
const Joi = require('joi');
const mongoose = require('mongoose');

// Mongoose schema
const projectSchema = new mongoose.Schema({
    name: String
});

// Mongoose model out of a schema
const Project = mongoose.model('Project', projectSchema);

// Joi validation
function validateProject(project) {
    const schema = Joi.object({
        name: Joi.string().min(1).max(10).required()
    });
    
    return schema.validate(project);
}

// Export module
module.exports.Project = Project;
module.exports.validateProject = validateProject;
```

### Validate using schema
```js
// Import Project model
const { Project, validateProject } = require('../models/project');

// Use validation somewhere in code
router.post("/", async (req, res) => {
    // Search for invalid values
    const { error } = validateProject(req.body);
    
    // If there are some, return message about the first one and close request - do not forget to "return", otherwise code will continue in execution
    if (error) return res.status(400).send(error.details[0].message);

    // If not, continue:
    // ...save project to database
});
```