# Javascript
## CONSUMING PROMISES

`const cart = ["shoes", "kurta"];`
// api create order
createOrder(cart);
// another api
proceedToPayment(orderId);
// so these both are async 
/////
// traditionally used to do using callback functions

// Designing functions in such a way,
// responsibility of create order API; once order is created
// such as 
`createOrder(cart, function (orderId) {
    proceedToPayment(orderId);
})`
// but there is an issue here; inversion of control
// means we have passed this callback function; that at some point
// Inversion of control (IoC) is a design principle in software engineering where the control over a particular process is transferred
// to an external component or framework. 
// In the context of asynchronous JavaScript, this often involves passing callbacks to functions, which are then executed when the asynchronous operation is complete.

createOrder(cart) // it will return a promise 
const promise = createOrder(cart); // this API is an async operation
// it will return {data: undefined} () // initially 
// whenever JS will execute the line 23, the createOrder API will 
// return an empty object and the program will just go on executing, and after 5 sec
// or any sec, the promise object will fill this data automatically

`// Once we have this data, 
// we will attach a callback function to this promise object
promise.then(function (orderId) {
    proceedToPayment(orderId)
}); // Promises give us this trust that as soon as we have this data, it will call this function once, and the control remains with us `

////////////////////////////////////

const GITHUB_API = "";
fetch() // it's an API given by the browser
// this fetch function returns us a promise
const user = fetch(GITHUB_API);

// PROMISE STATE
// STATE OF A PROMISE => RESULT 
// => PROMISE STATE => INITIALLY => PENDING STATE 
// => FULFILLED

`// WHAT IF HAD A CALLBACK FN ATTACHED 
user.then(function (data) {
    console.log(data);
})
// PROMISE OBJECTS are immutable`

//////////////////////
### What is a promise?
// A promise object is a placeholder for an async operation. By the time we get the value,
// it acts as a container for the future value
// A promise is an object representing the eventual completion or failure of an async operation (IMPORTANT DEFINITION)

/// One more issue with callbacks, such as callback hell

// To make it a bit complicated, what if after payment is done, you have to show Order summary by calling api.showOrderSummary() and now it has a dependency on api.proceedToPayment() Now my code should look something like this:

api.createOrder(cart, function () {
    api.proceedToPayment(function () {
        api.showOrderSummary();
    });
});
// Now what if we have to update the wallet, now this will have a dependency over showOrderSummary

api.createOrder(cart, function () {
    api.proceedToPayment(function () {
        api.showOrderSummary(function () {
            api.updateWallet();
        });
    });
}); // Instead of growing vertically, it starts to grow horizontally
// 💡 Callback Hell
// When we have a large codebase and multiple APIs and have dependencies on each other, then we fall into callback hell. These codes are tough to maintain. These callback hell structures are also known as the Pyramid of Doom.

// It is handled using Promise chaining

const promise = createOrder(cart);

promise.then(function (orderId) {
    proceedToPayment(orderId);

})

// or the above can also be written as 
createOrder(cart).then(function (orderId) {
    proceedToPayment(orderId);

})
.then(function (paymentInfo) {
    showOrderSummary(paymentInfo);

})
.then(function (paymentInfo) {
    updateWallet(paymentInfo)
})

// ALWAYS REMEMBER TO RETURN A PROMISE FROM A PROMISE, TO PASS ON THE VALUE,
// TO GET THE DATA PROPERTY IN THE CHAIN

createOrder(cart).then(function (orderId) {
    return proceedToPayment(orderId);

})
.then(function (paymentInfo) {
    return showOrderSummary(paymentInfo);

})
.then(function (paymentInfo) {
    return updateWallet(paymentInfo)
})

// Using arrow function; looking a more lean
createOrder(cart)
    .then(orderId => proceedToPayment(orderId))
    .then(paymentInfo => showOrderSummary(paymentInfo))
    .then(paymentInfo => updateWalletBalance(paymentInfo))
