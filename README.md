# jestjs Notes

## Testing 

> testing means checking that our code meets some expectations.

## Jest

>Jest is a JavaScript test runner, that is, a JavaScript library for creating, running, and structuring tests.
>Using NPM package, we can install it in any JavaScript project. Jest is one of the most popular test runner these days, and the default choice for React projects.

## Setting up the project

> * Make directory
> * npm init -y
> * npm install --save-dev jest
``` jsx harmony
 "scripts": {
    "test": "jest"
  },
```

## First Example

>This test used expect and toBe to test that two values were exactly identical.

``` jsx harmony
 function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

``` jsx harmony
 const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```
> npm run test (to run)


