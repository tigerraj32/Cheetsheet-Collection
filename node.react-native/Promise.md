The Promise.all() static method accepts a list of Promises and returns a Promise that:

- resolves when every input Promise has resolved or
- rejected when any of the input Promise has rejected.

The following shows the syntax of the Promise.all() method:

    Promise.all(iterable);



The iterable argument is a list of the promises passed into the Promise.all() as an iterable object.

If all the input promises resolve, the Promise.all() static method returns a new Promise that resolves to an array of resolved values from the input promises, in an iterator order.

If one of the input promises rejects, the Promise.all() returns  a new Promise that rejects with the rejection reason from the first rejected promise. The subsequent rejections will not affect the rejection reason. The returned Promise also handles the rejections silently.

The Promise.all() is useful when you want to aggregate the results from multiple asynchronous operations.

JavaScript Promise.all() examples

Let’s take some examples to understand how the Promise.all() works.

**1) Resolved promises example**

The following promises resolve to 10, 20, and 30 after 1, 2, and 3 seconds. We use the setTimeout() to simulate the asynchronous operations:

 ```javascript 
 
 const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');

        resolve(10);
    }, 1 * 1000);

});
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has resolved');
        resolve(20);
    }, 2 * 1000);
});
const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The third promise has resolved');
        resolve(30);
    }, 3 * 1000);
});
```


To wait for all three promises to resolve, you use the Promise.all() method:

```javascript
 Promise.all([p1, p2, p3])
    .then(results => {
        const total = results.reduce((p, c) => p + c);

        console.log(`Results: ${results}`);
        console.log(`Total: ${total}`);
    });
```
Output

    The first promise has resolved
    The second promise has resolved
    The third promise has resolved
    Results: 10,20,30
    Total: 60
    
When all promises have resolved, the values from these promises are passed into the callback of the then() method as an array.

Inside the callback, we use the Array’s reduce() method to calculate the total value and use the console.log to display the array of values as well as the total.

**2) Rejected promises example**

The Promise.all() returns a Promise that is rejected if any of the input promises are rejected.

 ```javascript
 const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');
        resolve(10);
    }, 1 * 1000);

});
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has rejected');
        reject('Failed');
    }, 2 * 1000);
});
const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The third promise has resolved');
        resolve(30);
    }, 3 * 1000);
});


Promise.all([p1, p2, p3])
    .then(console.log) // never execute
    .catch(console.log);
    
```


Output:

    The first promise has resolved
    The second promise has rejected
    Failed
    The third promise has resolved

In this example, we have three promises: the first one is resolved after 1 second, the second is rejected after 2 seconds, and the third one is resolved after 3 seconds.

As a result, the return promise is rejected because the second promise is rejected. The catch() method is executed to display the reason for the rejected promise.
