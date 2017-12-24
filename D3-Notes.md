# Learnings and Tricks

```js
d3.csv("data.csv",type, function(anArrayOfObject) {
  anArrayOfObject.forEach(function(d) {
      console.log(d.x + d.y);
    })
  });

// presume the data.csv to contain data like following
/*
x,y
1,2
4,5
8,2
5,9

When it read the data it reads them as a string. So the x and y values will be string and need to be cast into numbers
So it is common to find callbacks functions like following used in d3 a lot.

*/

// convert the x and y values of object d to number and assign it back to the object.
function type(d) {
  d.x = parseFloat(d.x);
  d.y = parseFloat(d.y);
  return d;
}
// same as above, the "+" is used for casting it to number before reassigning.
 function type(d) {
   d.x = +d.x;
   d.y = +d.y;
   return d;
 }
```

## Scales

```js
//In D3 we use scale functions to convert our data space to pixel spaces.
//d3.scale.linear() will return an instance of a scale.
var scale = d3.scale.linear();
//now we can set our data range of our dataset and pass it as an array [min, max]
scale.domain([0,1]); // Data space
scale.range([0,100]); //pixel space

//after configuring above, here you see when we call the scale function and pass the domain value, it returns the data value in the pixel range.
console.log(scale(0)); // prints 0
console.log(scale(0.5)); // prints 50
console.log(scale(1)); //prints 100
```
## Method chaining in D3

In D3 you can chain all the methods and call them one after the another. So the above code could have been written as

```js
// All could be called right after another.
var scale  = d3.scale.linear().domain([0,1]).range([0,100]);

console.log(scale(0)); // prints 0
console.log(scale(0.5)); // prints 50
console.log(scale(1)); // prints 100

```
## getters and setters
We can also call the domain and range functions without any arguments and we will get the values that have been set.

```js
var scale  = d3.scale.linear().domain([0,1]).range([0,100]);

console.log(scale.domain()); // prints [0,1]
console.log(scale.range()); // prints [0,100]
```

## Ordinal scales
These deal with data that have discrete values

```js
var scale = d3.scale().ordinal().domain(['A','B','C']).range("Apple", "Banana", "Coconut");

console.log(scale('A')); // prints Apple
console.log(scale('B')); // prints Banana
console.log(scale('C')); // prints Coconut

```
