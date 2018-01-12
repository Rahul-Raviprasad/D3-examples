# SVG

It is similar to html , but is meant for drawing. so the elements would be like
rect, circle, ellipse, polygon, path etc

There are only a handful of basic elements
1. line
2. rect

```html
<rect x="10" y="10" width="500" height="500" fill="yellow" stroke="blue" stroke-width="5" />
```
3. circle
4. ellipse
5. polygon
```html
<polygon points="32.5,66.7,34,56,87.44,99.0" fill="green" stroke="blue" stroke-width="5"/>
```
6. path
7. polyline

#### Linear gradient
It is a good practice to add id to the gradient as it help in usage.

```html
<linearGradient id="gradientId" gradientUnits="userSpaceOnUse" x1="0" x2="0" y1="0" y2="100%">
  <stop offset="0" style="stop-color:#88ff3f" />
  <stop offset="0.56" style="stop-color:#6Bd819">
  <stop offset="1" style="stop-color:#58BF00">
</linearGradient>

<!--using the gradient-->
<ellipse cx="150" cy="50" rx="150" ry="50" fill="url(#gradientId)" />
<path d="55,67 90,45" fill="url(gradientId)"/>

```

 example circles

```html
 <svg width="600" height="200">
   <circle cx="100" cy="100" r="5"></circle>
   <circle cx="200" cy="100" r="10"></circle>
   <circle cx="300" cy="100" r="20"></circle>
 </svg>
```

sometimes we would like to move our svg elements together, then we should the group tag "g"

```html
<!--group of elements which go together -->
<g transform="translate(100,50)">
  ....
</g>
```

## Using tools
Many online tools allow you to draw like traditional paint and then download the svg file to be used later.
Tools:-
https://vectr.com/
