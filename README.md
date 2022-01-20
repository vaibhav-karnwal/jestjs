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
###### npm run test (to run)
###### expect(1+2)---> returns a expectation object 
###### .tobe(3)----> is a matcher. tobe uses Object.is to test exact equality. If we want to check the value of an object, use toEqual

``` jsx harmony
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```
###### toEqual ---> recursively checks every field of an object or array.

###### You can also test for the opposite of a matcher:

``` jsx harmony
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
});
```



