Callbacks makes asynch programming possible. for eg. passing a function as callback to setTimeout, to call it after 5000ms.

#Callback hell:
lets understand callback hell with an example.
Suppose we have added some items to our cart,
Now, OrderCreation api is called --> after  that proceed to payment api should be called --> after that only the showOrderSummary api should be called --> and the update wallet.
So we will pass the later APIs as callback to the previous ones so that they will be called only after the previous ones are executed.

```javascript
const cart = ["shoes", "pants", "kurta"];
//this is known as Pyramid of doom
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
```
problem with the above code is Callback hell : So when we ahve large codebase and there are so many APIs dependent on one after the other, we wend up falling in this callback hell.
Callback hell: When their is one callback inside another callback inside another callback inside another callback or if statements and some random things happening makes this call back and our code starts to grow horizontally instead of vertically, this is known as callback hell.

#Inversion of control: Another problem we see while using callbacks
IOC is like when you lose the control of your code when we are using callbacks.
But how do we lose control over our code?
In below code we gave the control of our program to create order API, now its the responsibility of createOrder API to call our proceddTopayment() back, and we are blindly trusting createOrder API, now we don't know create order API must have been in some other service,or sonme other Dev wrote it or an intern wrote it, there could be a lot of bug inside it, what if our callback funtion was never called, we don't know what is the code written inside createORderAPI, what if our callback function is called twice. SO here we are giving the control of our piece of written code to some other code and we don't know what is happening behind the scene.

```javascript
const cart = ["shoes", "pants", "kurta"];
//this is known as Pyramid of doom
api.createOrder(cart, function () {

     api.proceedToPayment()
})
```