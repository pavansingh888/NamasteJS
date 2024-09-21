1.Functional programming is possible due to Higher order funtions.
2.Higher order functions are functions that take another function as an argument or return a function. on the other hand the function which is passed as an argument is known as Callback function.
3.Preffered way to write code using functional programming
* More modular
* reusable
* cleaner

Example 1-Funtional programming
```javascript

const radius = [3, 1, 2, 4];

const area = function(radius) {
return Math.PI * radius * radius;
};

const cicumference = function(radius) {
return 2 * Math.PI * radius;
};

const diameter = function(radius) {
return 2 * radius;
};

const calculate = function(radius, logic) {
const output = [] ;
for (let i = 0; i < radius.length; i++) {
output.push(logic(radius[i]));
}
return output;
}
console.log(calculate(radius, area) );
console.log(calculate(radius, cicumference) );
console.log(calculate(radius, diameter) ) ;
```

Example 2-Creating our own map function using Prototype.
```javascript
const radius = [3, 1, 2, 4];

const area = function(radius) {
return Math.PI * radius * radius;

};

const cicumference = function(radius) {
return 2 * Math. PI * radius;
};

const diameter = function(radius) {
return 2 * radius;
};

Arrays.prototype.calculate = function(logic) { //
const output = [] ;
for (let i = 0; i < radius.length; i++) {
output.push(logic(this[i])); //
}
return output;
}


console.log(radius.map(area)); 
console.log(radius.calculate(area)); //
// console.log(calculate(radius, cicumference) );
// console.log(calculate(radius, diameter) ) ;
```