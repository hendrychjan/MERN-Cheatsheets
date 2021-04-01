# Unit testing

## Install dependencies

`npm i jest --save-dev`

> Make sure to use "--save-dev" flag to install package as a development dependency!

## Register tests

- in `/package.json` edit `scripts` object as follows:

```json
"scripts": {
    "test": "jest"
  },
```

- optionally, you tell `jest` to run tests again automatically everytime you make change in your source code, so that you dont have to run tests manually:

```json
"scripts": {
    "test": "jest --watchAll"
  },
```

- this will tell npm to use `jest` as testing tool
  > now, you can execute tests by running `npm test`

## Write tests

- in `/tests` folder, create any test you want, following this naming pattern: `<test_name>.test.js`

### Test syntax

- remember to group all related tests together, to keep the test code clean and maintainable
- to write tests, use such syntax:
  > Matcher functions documentation: [here](https://jestjs.io/docs/en/using-matchers)

```
describe("<test name>", () => {
    it("<test description>", () =>{
        const result = <get result that you want to test>;
        expect(result).<matcher function>(<expected result>);
    });

    // ...and place all other related test here
});
```

### Test example

```js
const lib = require("../helpers/lib");

describe("absolute", () => {
  it("should return a positive number if input is positive", () => {
    const result = lib.absolute(1);
    expect(result).toBe(1);
  });

  it("should return a positive number if input is negative", () => {
    const result = lib.absolute(-1);
    expect(result).toBe(1);
  });

  it("should return 0 if input is 0", () => {
    const result = lib.absolute(0);
    expect(result).toBe(0);
  });
});
```
