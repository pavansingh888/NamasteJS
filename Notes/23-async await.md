What is async ?
What is await ?
How async await works behind the scenes?
Examples of using async/await
Error Handling
Interviews
Async await vs Promise. then/.catch

# What is async ?
async - is a keyword that is used before a function to make a function async.
***async function always returns a promise. here arises two cases:
1. When we return a promise from async function directly.
2. When we return any value, async function will automatically wrap the value inside a promise and will return a promise only.
```javascript
const p=new Promise((resolve,reject) => {resolve("Promise resolved value!!")}) //creating seperate promise to return it from inside async fn.
async function getData(){
   // return "Pavan";
    return p;
}

const dataPromise =getData();
// console.log(dataPromise); //logs a promise
dataPromise.then((res)=> console.log(res)); //logs returned wrapped value inside promise by accessing it using .then
```

# What is await ?

## async with await and how its different .then to handle promise?

1. async and await combo is used to handle promises 
2. but before async await how do we used to handle promises, do we really need async await? let compare both:
3. **await can only be used inside an async function & we write await infront of a promise and it resolves the promise**.
4. Promise is usualy something which doesn't resolve instantaneously. so lets use setTimeout to mimic that. To see the difference between older and and new async await way of handling promise.

```javascript
const p=new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve("Promise resolved value!!")
    },10000)
}
)

//How promise is handled using async await :

//we will use 'await'  keyword infront of a promise that has to be resolved.
//JS Engine will actually wait at the await line for the promise to be resolved.
async function handlePromise(){
   const val = await p; //val will store promise resolved value.
    console.log("Hey JS await")
    console.log(val, "new"); //Promise resolved value!!

    // will our program wait 2 times, how will it work? - After 10 seconds both above and below promises are resolved because both are awaiting for same promise to be resolved. So all 4 logs will be printed just after 10 seconds once promise p gets resolved.
    const val2 = await p; //val will store promise resolved value.
    console.log("Hey JS await2")
    console.log(val2, "new2"); //Promise resolved value!!
}

handlePromise();


//How promise was handled before to get the value :

//Here JS Engine will not wait for promise to be resolved
//lot of JS devs used to think that JS will wait at .then for the promise to be resolved.
 function getData(){
     p.then(res => console.log(res, "old")); //Promise resolved value!!
     console.log("Hey JS! .then") //printed before above line
 }

getData();


```



# Concept - lets create 2 different Promises now

## Case-1 : p2 resolves before p1
What will happen here is since p1 wait 10s and p2 wait 5s, so our program will wait for p1 that is 10s , but until then p2 would have resolved but our program will only proceed to further code once p1 is resolved. So when p1 gets resolved all the logs gets printed all at once because by the time our executor reaches p2, it is already resolved , hence futher logs gets printed instantly.

```javascript
//p1 wait 10s and p2 wait 5s
const p1=new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve("Promise resolved value!!")
    },10000)
}
)

const p2=new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve("Promise resolved value!!")
    },5000)
}
)

async function handlePromise(){
   const val = await p1; 
    console.log("Hey JS await")
    console.log(val, "new"); 

    
    const val2 = await p2;
    console.log("Hey JS await2")
    console.log(val2, "new2"); 
}

handlePromise();
```


## Case-2 : p1 resolves before p2
Here since p1 will be resolved in 5s - its logs will be printed. and after 5 more seconds that is 10s in total p2 will get resolved - then p2 logs will be printed. Since till p1 resolves, 5 seconds would have already been passed, and now at await p2 program will only need to wait for remaining 5 seconds.

```javascript
const p1=new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve("Promise resolved value!!")
    },5000)
}
)

const p2=new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve("Promise resolved value!!")
    },10000)
}
)

async function handlePromise(){
    console.log("Hellow world");
   const val = await p1; 
    console.log("Hey JS await")
    console.log(val, "new"); 

    
    const val2 = await p2;
    console.log("Hey JS await2")
    console.log(val2, "new2"); 
}

handlePromise();
```

### How async await works behind the scenes?
lets see how above code will execute in theory:
1. at 1st call stackk is empty, as soon as handlPromise() is called it see's that there are 2 promises that needs to be resolved.
2. because JS is Synch SIngle threaded language, it start executing line by line and log first log then it'll see 'await p1'
Will this handlePromise stay here and wait for the promise to be resolved? 
No , JS wait for nothing. And this **handlePromise() execution will 'Suspend' and this will move out of call stack and it will not block the main Thread so that any other events are happening they can just execute inside call stack.**
3. Now its suspended on await p1, **once this p1 is resolved then only it will move ahead in handlepromise() code. So after 5s once p1 is resolved handlePromise() will again come inside call stack and will start executing from where it left. Basically behind the scene JS will be tracking all this.**
4. In further execution once it reaches p2, it see's that p2 is still not resolved since it has been only 5s and p2 will resolve after 10s. So it will again suspend at await p2 until p2 is resolved and this way it will not block the call stack. 
5. after p2 is resolved, handlePromise() will again come into the callstack and will start executing from where it left.



# Real world Examples of using async/await
## Using fetch
```javascript
const API_URL = "https://api.github.com/users/pavansingh888";
 async function handlePromise(){
     const data = await fetch(API_URL);
     const jsonVal = await data.json();

     //fetch() => promise => when its resolved => we get response object inside our 'data'. => this response object have a body which is a readable stream => to convert this reponse body( which is a readble stream) , we will convert it to JSON => we will do response.json() or data.json() 
     // data.json() is again a promise. so we write await in front this promise, and whatever we will get from this will be the json value.
     console.log(jsonVal);
 }

handlePromise()
```


# Error handling
1. While we were using normal 'promise.then', we had a catch() method to handle errors.
But for async await, we will use try-catch methods.
2. Wherever we have async await inside our code we should wrap it inside try{} block and then we can have catch(err){console.log(err)} where we can handle error.
## Using try-catch
```javascript
const API_URL = "https://api.github.com/users/pavansingh888";
const InVALID_URL = "https://invalidrandomurl/";
console.log("Pavan")
 async function handlePromise(){
     try {
           const data = await fetch(InVALID_URL);
           const jsonVal = await data.json();
           console.log(jsonVal);
     } catch (error) {
         console.log(error);//TypeError: Failed to fetch
     }
 }
handlePromise()
```

## Using promise.catch()
```javascript
const API_URL = "https://api.github.com/users/pavansingh888";
const InVALID_URL = "https://invalidrandomurl/";
console.log("Pavan")
 async function handlePromise(){
     try {
           const data = await fetch(InVALID_URL);
           const jsonVal = await data.json();
           console.log(jsonVal);
     } catch (error) {
         console.log(error);//TypeError: Failed to fetch
     }
 }
handlePromise().catch((err)=> console.log(err));
```

# Interview + RECAP
Async is keyword which is used with functions and async functions are a different thing.
Await can be used only inside async functions to handle promises.
Then explain with example.
## What should we use async/await vs Promise.then/catch?
async await is just a syntactic sugar over promise.then.catch.
async/await is preferred and new way of writing async code, easier to use and read also. So use it.
