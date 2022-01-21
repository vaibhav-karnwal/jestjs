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

## expect(value)

>We often need to check whether the value meet the required condition or not, So in that expext function is used that gives the access to the matchers to test the values. Lets say there is a method sum(1,2) which is supposed to return the value 3.

```jsx harmony
test("1+2 is 3",()=>{
  expect(sum(1,2).tobe(3));
}
```

## expect.extend(matchers)

>We can use expect.extend to add our own matcher to jest. Lets say we are testing a number whether it comes in the range of other numbers or not.

```jsx harmony
expect.extend({
    toBeWithInRange(number,floor, ceil){
        const check = number>=floor && number<=ceil;
        if (check){
            return {
                message:()=>`expected ${number} not to be within range ${floor}-${ceil}`,
                pass: true,
            }
        }else{
            return{
                message:()=> `${number} to be within range ${floor}-${ceil}`,
                pass:false,
            }
        }
    }
})

test('number comes in range or not',()=>{
    expect(100).toBeWithInRange(90,110);
    expect(100).not.toBeWithInRange(90,99);
})
```

### Async Matchers

>expect.extend also supports async matchers. Async matchers return a promise that we have to wait for the retured value.

```jsx harmony
expect.extend({
    async toBeWithInRange(number,floor, ceil){
        const check = number>=floor && number<=ceil;
        if (check){
            return {
                message:()=>`expected ${number} not to be within range ${floor}-${ceil}`,
                pass: true,
            }
        }else{
            return{
                message:()=> `${number} to be within range ${floor}-${ceil}`,
                pass:false,
            }
        }
    }
})

test('number comes in range or not',async ()=>{
    await expect(100).toBeWithInRange(90,110);
    await expect(100).not.toBeWithInRange(90,99);
})
```















