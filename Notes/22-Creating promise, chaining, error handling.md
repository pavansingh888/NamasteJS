
```javascript
const cart = ["shoes" , "pants" ,"kurta"]

const promise= createOrder(cart); //orderId
// console.log(promise);

promise.then(function(orderId){
  console.log(orderId);
  return orderId; 
  //ProceedTopayment(orderId);
})
.catch(function(err){ /// will be called if above promise is rejected
  console.log(err.message);
})
.then(function(orderId){  //will still be even after previous catch or then.
    // console.log(err); //will print 'undefined for both err, orderId'
    return proceedToPayment(orderId); //returns a promise
})
.then(function(paymentInfo){
    console.log(paymentInfo);
})
.catch(function(err){
    console.log(err.message);
})
.then(function(orderId){
    console.log(orderId); //undefined
    console.log("No matter what happend, i will definetely be called") //same
})


//promise Producer

function createOrder(cart){

    //Resolve and reject are the functions which are given by javascript to build promises, its not something we pass in, it is given by JS by design of promise API.
    //How we can create our own promise?
    const pr = new Promise(function (resolve, reject){
     
      //validateCart -  if our cart is not valid we will reject the promise and will pass the error.
      if(!validateCart(cart)){
        const err= new Error("Cart is not valid")
        reject(err); 
        //rejecting with error
        //this will lead to red uncaught error  in dev console, but user  will not see anything and will not be notified about this error on its on until he open console.
        //So we always handle error gracefully by using catch() statements which can catch error if the promise is rejected, we can give a call back inside the catch(callback), which will be called to do something if promise is rejected.
      }

      //logic for create order - if cart is validated then we will create the order and will resolve the promise and will also pass the orderId.
      const orderId="12345";
      if(orderId){
        setTimeout(function(){
            resolve(orderId);
        },5000)
      }

    })

    //returning our promise
    return pr;
}

//validate cart function which returns either true or false
function validateCart(cart){
    // console.log(cart);
    return true;
    // return true;
    //if false will reject thye promise.
}

//proceed to paymet
function proceedToPayment(orderId){

    return new Promise(function(resolve,reject){
        console.log("ptp called")
        
        reject(new Error("Reject Payment is succesfull"));
        // resolve("Payment is succesfull");
    });
}
```