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
