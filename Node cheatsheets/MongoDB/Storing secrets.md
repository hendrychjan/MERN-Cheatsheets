# Storing application secrets

### Dependencies:

`npm i config`

### Map configuration

- first, you need to create a folder `config` in the root of your application
- inside this folder, create a json file `default.json` that will contain all the default private configuration data
- now, use this json file to specify a template for all the private data:
  > I used jwt as an example...

```json
{
  "jwtPrivateKey": ""
}
```

- now, create a new file `custom-environment-variables.json` inside `config` folder
- this file will define a mapping between configuration variables and ENV
- here, following syntax `"<key>": "<project_name>_<key>"` you have to define all the mapping, just as is show below:

```json
{
  "jwtPrivateKey": "myapp_jwtPrivateKey"
}
```

### Store data in ENV

- ENV (= environment variable) is a variable stored in your system
- you can set it like so: (replace values inside <> with your own data)
  - Windows CMD: `set <var_name>=<value>`
    - example: `set jwtKey=se4dws48d`
  - Windows PowerShell: `$env:<var_name>="<value>"`
    - example: `$env:jwtKey="se4dws48d"`
  - macOS: `export <var_name>=<value>`
    - example: `export jwtKey=se4dws48d`
  - Linux: `<var_name>=<value>`
    - example: `jwtKey=se4dws48d`

### Retrieve configuration data

- never forger to check whether the ENV exists at first, otherwise the application might not work properly:

  - in node.js, it is a good practise to do so in `index.js`

  ```js
  const config = require("config"); // import "config" package

  // check whether ENV exists
  if (!config.get("jwtPrivateKey")) {
    console.error("FATAL ERROR: jwtPrivateKey is not defined.");
    process.exit(1); // in case it doesnt, exit the application
  }
  ```

- use `config` package to retrieve your config data
- syntax: `config.get('<key>');`

```js
// import config package somewhere
const config = require("config");

// get config data
const key = config.get("jwtPrivateKey");
```
