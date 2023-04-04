
# Javascript debugging beyond console.log

One of the easiest ways to debug anything in JavaScript is by logging stuff using console.log. But there are a lot of other methods provided
by the console that can help you debug better. Some of them are below


- **console.log()**
```
console.log('Is this working?');
```

Output
> Is this working?

```javascript
const foo = { id: 1, verified: true, color: 'green' };
console.log('Is this working?');
```
Output
> { id: 1, verified: true, color: 'green' }

- **console.table**
Whenever we have objects array with common properties, then we can use `.tables()` to display the values in tabular form.
```javascript
let  arr  = [{"id": 1, "name":'rajan'}, {"id": 1, "name":'rajan'}]
console.table(a)
```

Output

|id|name|
|---|---|
|1|rajan|
|2|sanjay|


- **console.group**

This can be used when you want to group or nest relevant details together to be able to easily read the logs.
```javascript
console.group('User Details');
console.log('name: John Doe');console.log('job: Software Developer');
console.groupEnd()
```


- **console.time**
We can use `.time()` to easily show the amount of time taken to execute certain part of code.

```javascript
console.time('time1')
for (i = 0; i < 1000000; i++) {  
// For Loop}console.timeEnd("For loop");
}
console.timeEnd('time1')
```
Output
> time1: 4ms


