# Learnings and Tricks

## Document traversal
We can select all the elements in d3 in a way very similar to jQuery.
d3.select("body")      = $("body")
d3.selectAll(".class") = $(".class")
d3.select("#myDiv")    = document.getElementById("myDiv")

d3.selectAll().filter()

## Few Data manipulation methods
.attr -> this adds attributes to the selected elements
.style -> this is used for adding css style
.append -> adds html element to the selection as a child


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

## Range points
This will automatically sub divide the data according to the domain.

```js
var scale = d3.scale().ordinal().domain(['A','B','C','D']).rangePoints([0,100]);

console.log(scale('A')); // prints 0
console.log(scale('B')); // prints 33.3333...
console.log(scale('C')); // prints 66.666....
console.log(scale('A')); // prints 100
```

if you want a crisp line you can use rangeRoundPoints which will round off the numbers

```js
var scale = d3.scale().ordinal().domain(['A','B','C','D']).rangeRoundPoints([0,100]);

console.log(scale('A')); // prints 0
console.log(scale('B')); // prints 34
console.log(scale('C')); // prints 67
console.log(scale('A')); // prints 100
```
## How to create DOM elements in D3

The select method takes

```js
var svg = d3.select("body").append("svg");

svg.attr("width", 250);
svg.attr("height", 250);

var rect = svg.append("rect");

rect.attr("x", 50);
rect.attr("y", 50);
rect.attr("width", 20);
rect.attr("height", 20);

```

## selectAll().data().enter().append() pipeline

Here we do another d3 trick. Where we try to select all the "rect" in the svg. But as you will see there are none. then data is added to this svg [1,2,3,4,5] followed  by enter and append("rect") methods. So what it does is if data is present then ignores it but if data is not present then it appends the rect for the data. As we know the svg didn't have any rect so rect is appended for all 5 data, hence drawing 5 rectangles on the screen.

```js
var data = [1,2,3,4,5];

var scale = d3.scale.linear()
            .domain(1,5)
            .range(0,200);

var svg = d3.select("body").append("svg");

svg.attr("width", 250);
svg.attr("height", 250);

svg.selectAll("rect")
   .data(data)
   .enter()
   .append("rect")
   .attr("x", function(d) { return scale(d);}) // generally written as .attr("x", scale)
   .attr("y", 50)
   .attr("width", 20)
   .attr("height", 20);
```


for the x attribute we are calling a function scale from a function which has the only purpose to call it. So we can replace it by scale directly like this .attr("x", scale)

Now to prove that the enter().append() works only on data which doesn't correspond to a element, see the following.

```js

var scale = d3.scale.linear()
            .domain(1,5)
            .range(0,200);

var svg = d3.select("body").append("svg");

svg.attr("width", 250);
svg.attr("height", 250);

function render(data, color) {
  svg.selectAll("rect")
     .data(data)
     .enter()
     .append("rect")
     .attr("x", function(d) { return scale(d);}) // generally written as .attr("x", scale)
     .attr("y", 50)
     .attr("width", 20)
     .attr("height", 20)
     .attr("fill", color);
}

render([1,2,3], "red");

render([1,2,3,4,5], "blue");

```

after the second render one might expect all the rectangles to be blue. But it isn't, because when we did selectAll("rect") the second time there were already 3 rect available on the svg. So enter() make append to execute only only on the last to data 4 and 5. That why you will see 3 red and 2 blue rectangles.


# Render with data binding, enter, update, and exit stages
The render could be written in a better way as follows

```js
function render(data, color) {
  // bind data
  var rects = svg.selectAll("rect")
                .data(data);

  // now this won't execute every time as render is called, so its more like a static phase
  // enter
  rects.enter().append("rect");

  // update
  rect.attr("x", scale)
  .attr("y", 50)
  .attr("width", 20)
  .attr("height", 20)
  .attr("fill", color)

  //exit
  rects.exit().remove();
}

render([1,2,3], "red");

render([1,2,3,4,5], "blue");

render([1,2], "green"); // now beacause of the exit function above only 2 rects will be drawn.

```

another example

```js
var svg =  d3.select('body').append("svg");
svg.attr("width", 250);
svg.attr("height", 250);

function render(data) {
  // Bind data
  var circles = svg.selectAll("circle").data(data);

  // Enter
  circles.enter().append("circle").attr("r", 10);

  // Update
  circles
    .attr("cx", function(d) {return d.x;})
    .attr("cy", function(d) {return d.y;});

  // Exit
  circles.exit().remove();
}

var myArrOfObjects = [
  {x: 100, y:100},
  {x: 130, y:120},
  {x: 80, y:180},
  {x: 180, y:80},
  {x: 180, y:40}
];

render(myArrOfObjects);
```

## Finding min and max in d3.

```js

d3.csv("dataFile.csv", type, function(data) {
  var min = d3.min(data, function(d) { return d.sepal_length;});
  var max = d3.max(data, function(d) { return d.sepal_length;});
  console.log(min, max);
});
```

## extent in d3
this does the same thing like min and max


## Adding classes through code
so when we want to add class to the element we are creating we can simply add attribute
```js
var svg = d3.select("body").append("svg");

svg.attr("height", 500).attr("width", 500).attr("class", "className1 classname2");

```
In above code the space separated classnames will be applied separately to the svg. We can use this to style normally as we do with CSS for other HTML elements.
