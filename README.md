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

###### We can also test for the opposite of a matcher:

``` jsx harmony
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
});
```

## Jest Helpers

Jest contains helpers that let you be explicit about what you want.

> * toBeNull matches only null
> * toBeUndefined matches only undefined
> * toBeDefined is the opposite of toBeUndefined
> * toBeTruthy matches anything that an if statement treats as true
> * toBeFalsy matches anything that an if statement treats as false

```jsx harmony
test('null', () => {
  const n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
  expect(n).not.toBeTruthy();
  expect(n).toBeFalsy();
});

test('zero', () => {
  const z = 0;
  expect(z).not.toBeNull();
  expect(z).toBeDefined();
  expect(z).not.toBeUndefined();
  expect(z).not.toBeTruthy();
  expect(z).toBeFalsy();
});
```

## Numbers

>tobe and toEqual are equivalent for numbers

```jsx harmony
test('two plus two', () => {
  const value = 2 + 2;
  expect(value).toBeGreaterThan(3);
  expect(value).toBeGreaterThanOrEqual(3.5);
  expect(value).toBeLessThan(5);
  expect(value).toBeLessThanOrEqual(4.5);

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4);
  expect(value).toEqual(4);
});
```

>For floating point numbers, toBeCloseTo is used instead of toEqual because we do not want error for small round figures.

```jsx harmony
test('adding floating point numbers', () => {
  const value = 0.1 + 0.2;
  //expect(value).toBe(0.3); This won't work because of rounding error
  expect(value).toBeCloseTo(0.3); // This works.
});
```

## Strings

> we use toMatch for checking string against regular expressions.
```jsx harmony
test('there is no I in team', () => {
  expect('team').not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/);
});
```
