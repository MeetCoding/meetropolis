# HTML

```html
<h1> Heading 1 </h1>
<h2> Heading 2 </h2>
<h3> Heading 3 </h3>
<h4> Heading 4 </h4>
<h5> Heading 5 </h5>
<h6> Heading 6 </h6>
<p> Paragraph </p>
```

```html
<tag attribute="value"> Text </tag>
```

```html
<a href="link"> Anchor Tag </a>
<img src="link" alt="text">
<ul> or <ol>
	<li> Item 1 </li>
	<li> Item 2 </li>
	<li> Item 3 </li>
</ul> or </ol>
```

```html
<input type="text" placeholder="Enter text">
<input type="email" placeholder="Enter email">
<input type="password" placeholder="Enter password">
<input type="number" min="1" max="10">
<input type="date">
<input type="file">
<input type="checkbox" id="agree">
<input type="radio" name="gender" value="male">
<textarea rows="4" placeholder="Enter message"></textarea>
<select>
  <option value="option1">Option 1</option>
  <option value="option2">Option 2</option>
</select>

```
# CSS
```css
tag { }
.class { }
#id { }
.class > * { } /* Children */
```

```css
(unit):
px, cm -> Absolute units
rem -> Root font size
vw/vh -> viewport width/height
vmin -> viewport smaller dimension
vmax -> viewport bigger dimension
```

```css
display: none | hidden | block;
background-color: (color);
width: (unit);
height: (unit);
min-width: (unit); 
max-width: (unit);
min-height: (unit);
max-height: (unit);
```

```css
animation-name: (name);
animation-duration: (time);
animation-delay: (time);
animation-iteration-count: (number);
time -> 3s, 500ms
number can be infinite
```
### Font Styling
```css
font-family: "Family Name";
font-style: italic;
text-decoration: underline;
font-size: (unit);
font-weight: (100-900);
text-align: center/left/right/justify;
color: (color);
```

```css
(color):
red, green, blue -> names
#ffffff -> hex value
rgba(red, green, blue, opacity)
hsl(hue, saturation, lightness)
```
### Box Model
```css
margin: (unit);
margin-inline: (unit); /*left right*/
margin-block: (unit); /*top bottom*/

padding: (unit);
padding-inline: (unit); /*left right*/
padding-block: (unit); /*top bottom*/

border: (unit) (style) (color);
style -> solid, dashed
```

```css
@keyframes name {
	0% { /* ... */ }
	50% { /* ... */ }
	100% { /* ... */ }
}
```
## Flexbox
```css
display: flex
flex-direction: row | column | row-reverse | column-reverse;
flex-wrap: wrap | nowrap;
justify-content: start | end | center | space-between | space-evenly; -> Main Axis
align-items: start | end | center | stretch; -> Cross Axis
flex-grow: (number); -> To be added on child
```

# Tailwind CSS

```
none | hidden | block
bg-(name)-(shade) -> shade: 50-950
w-(num) h-(num) or w-[x] h-[x]
maxw-(num) minw-(num)
maxh-(num) minh-(num)

(num): +1 = 0.25rem
	1-12 -> increament of 1
	12, 14, 16 -> increament of 2
	16, 20, 24... -> increament of 4
```

```
m-(num) mx-(num) my-(num)
p-(num) px-(num) py-(num)

border-(px)
border-(t/l/r/b)-(px)
border-(color)-(shade)
rounded-(xs/sm/lg/xl..9xl)
```

```
italic | non-italic
underline
font-thin | font-light | font-medium | font-semibold | font-bold | font-extrabold
text-left | text-center | text-right | text-justify
text-xs | text-sm | text-base | text-lg | text-xl ... text-9xl
text-color-(name)-(shade)
```

## Flexbox

```
flex
flex-row | flex-col | flex-row-reverse | flex-col-reverse
flex-wrap | flex-nowrap
justify-start | justify-center | justify-end | justify-between | justify-evenly
items-start | items-center | items-end | items-stretch
grow-[ . ] (Replace . by number)
```