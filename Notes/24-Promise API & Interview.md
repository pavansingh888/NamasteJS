# 4 Promise APIs:
Promise :
--> if settled (can be resolve or reject)
--> if settled (can be success or failure)
--> if settled (can be fulfilled or rejected)

## Promise.all()
Whats the contract for it? - to make parallel API calls and get the results. to get the result for 10 different users.
Used to handle multiple API call or multiple promises.
Its takes an array of promises as an input. (Most of the time its array) Generally it takes an iterable.

Promise.all([p1, p2, p3])
lets say we make 3 parallel API calls and get the result as 3 promises as an array.
lets say :
p1 takes 3s
p2 takes 1s
p3 takes 2s

### Case - 1 - all p's are success:
Promise.all() will return an array with result value of all three promises.
[val1, val2, val3] 
After 3s we will get this result since after 3s all will get fulfilled.
It will wait for all of them to finish.
### Case - 1 - One of the p's are get rejected:
As soon as any of the promises gets rejected Promise.all() will throw and error value of the rejected promise. - (Error)
lets say p2 gets rejected, therefore Promise.all() will give error immediately after 1s without waiting for other promises to get settled.
(other promises might get resolve or reject after 3s but our Promise.all() output  will be error only)

```javascript
const p1 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P1 Sucess"), 3000);
});

const p2 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P2 Sucess"), 1000);
//setTimeout(() >reject("P2 Fail"),1000);
});

const p3 = new Promise((resolve, reject) => {
//setTimeout(() => resolve("P3 Sucess"), 2000);
setTimeout(() => reject("P3 Fail"), 2000);
});
Promise.all([p1, p2, p3])
.then ((res) => {
console.log(res); //[P1 success,P2 success,P3 success]
})
.catch((err) => {
console.error(err); //P3 fail
});
```
What if we just want results from successful promises, we don't care if one of the promise fails. We just want result from the successfull ones. for that we have:


## Promise.allSettled()
It will wait for all the promise to get settled(fullfilled or rejected).
Output will always be an array with the same number of promises that we passed in.
Promise.allSettled([p1, p2, p3]).
lets say :
p1 takes 3s
p2 takes 1s
p3 takes 2s
lets say p2 gets rejected, p1 & p3 gets fullfilled so output will still be an array containing result value or error of respective promise. And this output will come after 3s as all promise takes 3s sec to get settled.
[val1, err2, val3] - this values will be object with status , value/reason of result/failure.

Usecase- suppose we want to show card based on 5 API calls and one of them gets rejected then just show the 4 cards.

```javascript
const p1 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P1 Sucess"), 3000);
});

const p2 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P2 Sucess"), 1000);
//setTimeout(() >reject("P2 Fail"),1000);
});

const p3 = new Promise((resolve, reject) => {
//setTimeout(() => resolve("P3 Sucess"), 2000);
setTimeout(() => reject("P3 Fail"), 2000);
});
Promise.allSettled([p1, p2, p3])
.then ((res) => {
console.log(res); //[P1 success,P2 success,P3 success]
})
.catch((err) => {
console.error(err); //P3 fail
});

//OUPUT:

// (3) [{…}, {…}, {…}]
// 0: {status: 'fulfilled', value: 'P1 Sucess'}
// 1: {status: 'fulfilled', value: 'P2 Sucess'}
// 2: {status: 'rejected', reason: 'P3 Fail'}
// length: 3
// [[Prototype]]: Array(0)
```

## Promise.race()
(Settle seeking race between promises)
As the name suggest, the person who finsishes first will be the winner.
It will return result of first settled(fullfilled value/result or Error) promise.

Promise.race([p1, p2, p3])
lets say :
p1 takes 3s
p2 takes 1s
p3 takes 2s
Here p2 will get settled after 1s second, So either its fullfilled (value2) is returned or if it gets rejected after 1s then (error) will be returned by Promise.race() without caring about other APIs.

```javascript
const p1 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P1 Sucess"), 3000);
});

const p2 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P2 Sucess"), 1000);
//setTimeout(() >reject("P2 Fail"),1000);
});

const p3 = new Promise((resolve, reject) => {
//setTimeout(() => resolve("P3 Sucess"), 2000);
setTimeout(() => reject("P3 Fail"), 2000);
});
Promise.race([p1, p2, p3])
.then ((res) => {
console.log(res); 
})
.catch((err) => {
console.error(err); 
});

//OUTPUT: P3 fail
```

## Promise.any()
(Success/fullfilled seeking race between promises)
Similar to Promise.race(), but instead of waiting for first promise to get settled as in case of Promise.race() - It will wait for the first promise to get successfull/fullfilled.
It will return (value) of first resolved ppromise. or if all promise get rejected/failed then output will be an Aggregated error which is an array [err1, err2, err3]

Promise.any([p1, p2, p3])
lets say :
p1 takes 3s
p2 takes 1s
p3 takes 2s
if p2 fails, then if p3 resolves then Promise.any() will return result (value3) of p3.
if all fails, returns list of errors - [err1, err2, err3]

### Case 1 - Any one is fulfilled
```javascript
const p1 = new Promise((resolve, reject) => {
setTimeout(() => resolve("P1 Sucess"), 3000);
});

const p2 = new Promise((resolve, reject) => {
// setTimeout(() => resolve("P2 Sucess"), 1000);
setTimeout(() =>reject("P2 Fail"),1000);
});

const p3 = new Promise((resolve, reject) => {
//setTimeout(() => resolve("P3 Sucess"), 2000);
setTimeout(() => reject("P3 Fail"), 2000);
});
Promise.any([p1, p2, p3])
.then ((res) => {
console.log(res); 
})
.catch((err) => {
console.error(err); 
});

// OUTPUT: P1 Sucess
```

### Case 2 - All promise are rejected
```javascript
const p1 = new Promise((resolve, reject) => {
setTimeout(() => reject("P1 fail"), 3000);
});

const p2 = new Promise((resolve, reject) => {
// setTimeout(() => resolve("P2 Sucess"), 1000);
setTimeout(() =>reject("P2 Fail"),1000);
});

const p3 = new Promise((resolve, reject) => {
//setTimeout(() => resolve("P3 Sucess"), 2000);
setTimeout(() => reject("P3 Fail"), 2000);
});
Promise.any([p1, p2, p3])
.then ((res) => {
console.log(res); 
})
.catch((err) => {
console.error(err);  //index.js:19 AggregateError: All promises were rejected
// to access aggrefate error array, we need to access 'errors' array from 'err' object
console.error(err.errors); 
// (3) ['P1 fail', 'P2 Fail', 'P3 Fail']
// 0: "P1 fail"
// 1: "P2 Fail"
// 2: "P3 Fail"
// length: 3
// [[Prototype]]: Array(0)
});

// OUTPUT: index.js:19 AggregateError: All promises were rejected
// (3) ['P1 fail', 'P2 Fail', 'P3 Fail']
// 0: "P1 fail"
// 1: "P2 Fail"
// 2: "P3 Fail"
// length: 3
// [[Prototype]]: Array(0)
```
