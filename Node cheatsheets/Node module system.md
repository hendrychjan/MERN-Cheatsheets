# Node module system
### Export module
- to export variable or function from module, you need to add them to ```module.exports``` object, just like so:
```js
module.exports.myFunction = myFunction;
```
### Import module
- to import module, use the ```require``` function as shown below:
```js
var importedFunction = require('./myModule'); //path to JS document
```