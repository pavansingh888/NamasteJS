#Promise Intorduction

```javascript
const cart = ["Shoes", "pants", "kurta"];

createOrder(cart, function(orderId) {
    porceedToPayment(OrderId);
});

//Better  approach using Promises:

//we would have designed our create order API in such a way that, createOrder wont take a callback but it'll just return us a promise
const promise = createOrder(cart);
//storing promise
//Promise - Its nothing but an empty object, which have some 'data' value in it - whatever the create order API will return to us.
//{data: 'whateever data returned to us by createorder API'} 
//at first it will just assume data of this object to be an empty value represented by 'undefined'.
//during all this the JS program will keep on executing further
//and after whatever time suppose 5-10 sec , createorderAPI will create an order, gives us order details and will fill this data value in the object back automatically.
//{data: OrderDetails} 
//What will happen after we get data from createOrderAPI? : Now we will attach our callback function proceedToPaymentAPI to this promise object using a '.then' function - this is the function which is available over promise object.
promise.then(function(orderId) {
    porceedToPayment(OrderId);
})
//Once we get data inside promise object, Callback fuhnction attached to the promise object will bwe automatically called.

//So here  there is a difference, earlier we were  passing the CB fx to createOrderAPI, but here we are attaching our CB fx to promise object. So in promise case, we will  have the control of our program with us, createOrderAPI will just create the order and fill the orderDetails in the promise object. 
```

#Promise object in Deep:
```javascript
const GITAPI="https://api.github.com/users/pavansingh888";
//by design fetch function returns us a promise
const user = fetch(GITAPI);
console.log(user);
//Promise {[[PromiseState]]: 'pending', [[PromiseResult]]: undefined, Symbol(async_id_symbol): 33, Symbol(trigger_async_id_symbol): 10}

user.then((data)=> console.log(data))
// _Response {Symbol(state): Proxy(Object), Symbol(headers): _Headers}

setTimeout(()=> console.log(user),3000);
//Promise {[[PromiseState]]: 'fulfilled', [[PromiseResult]]: _Response, Symbol(async_id_symbol): 33, Symbol(trigger_async_id_symbol): 10}
```

* 3 Promise states are - Pending, Fullfilled, Rejected
* Promise objects are immutable, we can just pass the data here and there in our code and we don't have to worry about that someone can mutate our data. We can't just edit 'user' using user.data.xx=something - not possible due to immutability.

```javascript
const cart = ["Shoes", "pants", "kurta"];

api.createOrder(cart, function () {
     api.proceedToPayment(function () {
        api.showOrderSummary(
              function() {
                   api.updateWallet()
              }
        )
    }
    )
})

//With Promise:
//** Its important to return the promise inside the promise chain, otherwise we might lose our data in further promise chain.
createOrder(cart)
 .then((orderId)=> {return proceedToPayment(orderID)})
 .then((PaymentInfo)=> {return showOrderSummary(PaymentInfo)})
 .then((PaymentInfo)=> {return updateWallet(PaymentInfo)})


promise.then(function(orderId) {
    porceedToPayment(OrderId);
})
 
```

#Interview Questions on promise:
* What is promises in JS?
Promise is an object that represents the eventual completion of an asychronous operation.
* Why promise is important?
Solves Callback hell, inversion of control


