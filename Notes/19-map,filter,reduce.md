All three are present in Array,prototype and used to apply on array.

1.Map- Maps each element using some logic to form and return a new array after applying that logic on each element.

```javascript
const products = [
    { name: 'Laptop', price: 1000 },
    { name: 'Phone', price: 500 },
    { name: 'Tablet', price: 700 }
];

const discount = 0.1;  // 10% discount

const discountedProducts = products.map(product => ({
    ...product,  // spread to keep other properties unchanged
    price: product.price * (1 - discount)
}));

console.log(discountedProducts);
/*
[
  { name: 'Laptop', price: 900 },
  { name: 'Phone', price: 450 },
  { name: 'Tablet', price: 630 }
]
*/
```

2.Filter- 
* filters each element using some logical condition on each element to form and return a new array after applying that condition on each element.
* filter expects a callback function, which checks the condition for each element and returns either T or F to filter that element based on condition.
```javascript
const jobs = [
    { title: 'Frontend Developer', experience: 3 },
    { title: 'Backend Developer', experience: 5 },
    { title: 'Full Stack Developer', experience: 7 }
];

const minExperience = 5;

const filteredJobs = jobs.filter(job => job.experience >= minExperience);

console.log(filteredJobs);
/*
[
  { title: 'Backend Developer', experience: 5 },
  { title: 'Full Stack Developer', experience: 7 }
]
*/
```

3.Reduce- 
* used to return a single output rather than an array.
* for example sum or max element of an array.
* Reduce expects a function whose first argument will be an accumulator(whose initial value we provide as a second argument of reduce function) and secound argument will be current element of the array on which reduce is applied.

```javascript
const orders = [
    { orderId: 'A001', total: 150 },
    { orderId: 'A002', total: 200 },
    { orderId: 'A003', total: 100 }
];

const totalRevenue = orders.reduce((acc, order) => acc + order.total, 0);

console.log(totalRevenue); // 450
```

# Chaining of all three
```javascript
const cart = [
    { name: 'Laptop', price: 1200, inStock: true },
    { name: 'Keyboard', price: 80, inStock: false },
    { name: 'Monitor', price: 300, inStock: true },
    { name: 'Mouse', price: 50, inStock: true }
];

const discountThreshold = 100;
const discountRate = 0.15;  // 15% discount on items over $100

const finalTotal = cart
    // 1. Apply discount to items over $100 using map
    .map(item => ({
        ...item,
        price: item.price > discountThreshold ? item.price * (1 - discountRate) : item.price
    }))
    // 2. Filter out items that are out of stock
    .filter(item => item.inStock)
    // 3. Reduce to calculate the final total price
    .reduce((total, item) => total + item.price, 0);

console.log(finalTotal); // 1275
```

