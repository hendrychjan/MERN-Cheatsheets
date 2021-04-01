# Implement bootstrap
### Install dependencies
- install bootstrap package:
    - ```npm i bootstrap@next``` for the newest version
    - ```npm i bootstrap@{{version}}``` for specific version
- if you want to use install bootstrap dependencies:
```
npm i popper.js
npm i jquery
```
### Import
- add imports to ```src/index.js```:
```javascript
import $ from 'jquery';
import Popper from 'popper.js';
import 'bootstrap/dist/js/bootstrap.bundle.min'; // JS part of bootstrap
import 'bootstrap/dist/css/bootstrap.min.css';   // CSS part of bootstrap
```

> Now, you can copy-paste code from [bootstrap](https://getbootstrap.com) as much as you want :smile: