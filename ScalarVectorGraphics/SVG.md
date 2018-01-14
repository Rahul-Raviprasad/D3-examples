# SVG

It is similar to html , but is meant for drawing. so the elements would be like
rect, circle, ellipse, polygon, path etc

One of the advantages of svg is that for simple graphics, the file size is smaller and the quality is higher(best of both worlds)
But there is a complexity limit

### When to consider SVG
* When the image or drawing is containing few basic shapes and colors - definitely go for SVG
* When you are doing fancy stuff with Div and Css to create basic shapes, we can easily build a ice-cream or console remote with html and css, but in such cases we should rather think of using a professional tool like illustrator or vectr to create them in SVG as it makes more sense and can be changed or we cn increase the complexity further.
* The image is cartoonish.. then we should compare the zipped file sizes and consider. -Maybe use SVG in such cases
* If the image is like a raster image and pixel by pixel things are complex then it makes no sense to use SVG.

## using SVG on the web
* SVG as image on html
```html
<img src="nerds.svg" alt="nerds being cool">
```

* SVG as background image in CSS
```CSS
.main {
  background-image: url(people.svg);
}
```
* inline SVG in html
like the most of the examples in this page.

## performance
SVG is pretty efficient already, but it can also be heavily optimized in multiple ways
* it gzips very well. because it has a lot of repetitive strings.
* svgo is great tool for optimizing by removing the least order numbers like 2.3345 is the 0.0045 accuracy really needed?


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

## <use> tag

https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use

## Using tools
Many online tools allow you to draw like traditional paint and then download the svg file to be used later.
Tools:-
https://vectr.com/

# resources
https://css-tricks.com/mega-list-svg-information/
jankfree.org
advanceperformance audits with dev tools by paul irish
weighting svg animation techniques with benchmarks
