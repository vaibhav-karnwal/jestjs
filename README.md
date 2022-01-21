# jestjs Notes

## Testing 

> testing means checking that our code meets some expectations.

## What is unit testing in React?

>Unit testing is a level of software testing where individual units/components of a software are tested. In the React world this means testing an individual React Component or pure functions.

## Jest

>Jest is a JavaScript test runner, that is, a JavaScript library for creating, running, and structuring tests. It's an open source project maintained by Facebook, and it's especially well suited for React code testing. Jest is very fast and easy to use.
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

### expect(value)

>We often need to check whether the value meet the required condition or not, So in that expext function is used that gives the access to the matchers to test the values. Lets say there is a method sum(1,2) which is supposed to return the value 3.

```jsx harmony
test("1+2 is 3",()=>{
  expect(sum(1,2).tobe(3));
}
```

### expect.extend(matchers)

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

#### Async Matchers

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

### expect.anything()

>expect.anything() match anything but not null and undefined value.We use it inside toEqual or toBeCalledWith instead of literal values.

```jsx harmony
test('map calls its argument with a non-null argument', () => {
    const mock = jest.fn();
    [1].map(x => mock(x));
    expect(mock).toBeCalledWith(expect.anything());
  });
```

### expect.any(constructor)

>expect.any(constructor) match anything that is created inside constructor. We use it inside toEqual or toBeCalledWith instead of literal values.

```jsx harmony
function randocall(fn) {
    return fn(Math.floor(Math.random() * 6 + 1));
  }
  
  test('randocall calls its callback with a number', () => {
    const mock = jest.fn();
    randocall(mock);
    expect(mock).toBeCalledWith(expect.any(Number));
  })
  ```

### expect.arrayContaining(array)

>expect.arrayContaining(array) match the received array with the expected array.

```jsx harmony
describe("match arrayContaining",()=>{
    const expected = ["vaibhav","sai"];
    it("match even if received contains additional elements",()=>{
        expect(["vaibhav","sai","vamsi"]).toEqual(expect.arrayContaining(expected))
    })
    it("does not match if received does not contain expected elements",()=>{
        expect(["karnwal","mohan"]).not.toEqual(expect.arrayContaining(expected))
    })
})
```

```jsx harmony
describe('Beware of a misunderstanding! A sequence of dice rolls', () => {
    const expected = [1, 2, 3, 4, 5, 6];
    it('matches even with an unexpected number 7', () => {
      expect([4, 1, 6, 7, 3, 5, 2, 5, 4, 6]).toEqual(
        expect.arrayContaining(expected),
      );
    });
    it('does not match without an expected number 2', () => {
      expect([4, 1, 6, 7, 3, 5, 7, 5, 4, 6]).not.toEqual(
        expect.arrayContaining(expected),
      );
    });
  });

```
### expect assertions(number)

>expect.assertions(number) verifies that a certain number of assertions must be called during the test. It is used when we are dealing with asynchronous code. In order to make sure that all callbacks to be called actually we used assertions

```jsx harmony


```

## Asynchronous Code

### callback


```jsx harmony
function fetchData(callback){
    setTimeout(()=>{
        callback("peanut butter");
    },1000)
}

test('the data is peanut butter', () => {
    function callback(data) {
      expect(data).toBe('peanut butter');
    }
  
    fetchData(callback);
  });
```
>it shows a problem that the test is completing as soon as fetchdata completed,not waiting for callback function to finish. 
>So, this problem is solved using done() function 

```jsx harmony
function fetchData(callback){
    setTimeout(()=>{
        callback("peanut butter");
    },1000)
}

test('the data is peanut butter', done=> {
    function callback(data) {
      expect(data).toBe('peanut butter');
      done();
    }
  
    fetchData(callback);
  });
  ```

### Promises

>This is a more straightforward way to handle asynchronous tests. Return a promise from the test, and Jest will wait for that promise to resolve. If the promise is rejected, the test will automatically fail.
>Lets say fetch data instead of using a callback returns a promise that is supposed to resolve the string"peanut butter"

```jsx harmony
function fetchData(){
    return new Promise((resolve)=>{
        setTimeout(()=>{
            resolve("peanut butter")
        },100)
    })
}

test("the data is peanut butter",()=>{
    return fetchData().then(data=>{
        expect(data).toBe("peanut butter");
    })
})
```
>If we will not use return statement then the test will complete before the promise returned from fetchData resolves.

### .resolves/.rejects

>we can also use .resolves matcher in our expect statement and Jest will wait for that promise to resolve. If the promise is rejected, the test will automatically fail.

```jsx harmony
test('the data is peanut butter', () => {
    return expect(fetchData()).resolves.toBe('peanut butter');
  });

function fetchData(){
    return new Promise((resolve)=>{
        setTimeout(()=>{
            resolve("peanut butter")
        },100)
    })
}
```
```jsx harmony
test('the fetch fails with an error', () => {
    return expect(fetchDataWithErrorMessage()).rejects.toMatch('error');
  });

function fetchDataWithErrorMessage(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            reject("error")
        },100)
    })
}
```

### Async/Await

> We can use async and await in our tests. To write an async test, use the async keyword in front of the function passed to test.

```jsx harmony
function fetchData(){
    return new Promise((resolve)=>{
        setTimeout(()=>{
            resolve("peanut butter")
        },100)
    })
}

test('the data is peanut butter', async () => {
    const data = await fetchData();
    expect(data).toBe('peanut butter');
  });
  ```
  ```jsx harmony
  function fetchDataWithErrorMessage(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            reject("error")
        },100)
    })
}

test('the fetch fails with an error', async () => {
    expect.assertions(1)
    try{
        await fetchDataWithErrorMessage();
    }catch(e){
        expect(e).toMatch('error');
    }
  });```
  
  ```jsx harmony
  function fetchData(){
    return new Promise((resolve)=>{
        setTimeout(()=>{
            resolve("peanut butter")
        },100)
    })
}

test('the data is peanut butter', async () => {
    await expect(fetchData()).resolves.toBe('peanut butter');
  });
 ```
 
 >We can combine async and await with .resolves or .rejects.
 
 ```jsx harmony
 function fetchDataWithErrorMessage(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            reject("error")
        },100)
    })
}

test('the fetch fails with an error', async () => {
    await expect(fetchDataWithErrorMessage()).rejects.toMatch('error');
  });
  ```
