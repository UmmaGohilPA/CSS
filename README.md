# web.dev/learn/css

## Box Model

- Boxes have different behavior based on their display value, their set dimensions, and the content that lives within them. You can control this by using extrinsic sizing, or, you can continue to let the browser make decisions for you based on the content size, using intrinsic sizing.

Example

```
// HTML
<figure class="box-demo box" data-element="parent-box">
  <img src="http://source.unsplash.com/CiFaYIvZyyA/800" alt="A purple Petunia flower in close focus" />
  <figcaption contenteditable>
    You can edit this text to see how it changes the layout of our box,
    depending on intrinsic and extrinsic sizing.
  </figcaption>
</figure>

// CSS
.box-demo {
  width: 400px;
  height: 400px;
}

.box-demo[data-sizing="intrinsic"] {
  width: max-content;
  height: max-content;
}

.box-layout {
  display: grid;
  width: max-content;
  gap: 0.5rem;
  grid-template-columns: 1fr min-content;
}
```

- When you switch to intrinsic sizing, you are letting the browser make decisions for you, based on the box's content size. It's much more difficult for there to be overflow with intrinsic sizing because our box will resize with its content, rather than try to size the content. It's important to remember that this is the default, flexible behavior of a browser. Though extrinsic sizing gives more control on the surface, intrinsic sizing provides the most flexibility, most of the time.

- Boxes are made up of four parts: margin box, border box, padding box and the content box

- You start with content box, which is the area that the content lives in. As you learned before: this content can control the size of its parent, so is usually the most variably sized area.

- The padding box surrounds the content box and is the space created by the padding property. Because padding is inside the box, the background of the box will be visible in the space that it creates. If our box has overflow rules set, such as overflow: auto or overflow: scroll, the scrollbars will occupy this space too.

- An inline element has block margin, but other elements won't respect it. Use inline-block, and those elements will respect the block margin, while the element maintains most of the same behaviors it had as an inline element.

- A block item will, by default, fill the available inline space, whereas a inline and inline-block elements will only be as large as their content.

- Having content-box as the value of box-sizing means that when you set dimensions, such as a width and height, they will be applied to the content box. If you then set padding and border, these values will be added to the content box's size.

## Selectors

- A universal selector—also known as a wildcard—matches any element.

```
* {
  color: hotpink;
}
```

- A type selector matches a HTML element directly.

```
section {
  padding: 2em;
}
```

- A HTML element can have one or more items defined in their class attribute. The class selector matches any element that has that class applied to it.

```
<div class="my-class"></div>
<button class="my-class"></button>
<p class="my-class"></p>

.my-class {
  color: red;
}
```

- An HTML element with an id attribute should be the only element on a page with that ID value.

```
#rad {
  border: 1px solid blue;
}

<div id="rad"></div>

```

- You can look for elements that have a certain HTML attribute, or have a certain value for an HTML attribute, using the attribute selector. Instruct CSS to look for attributes by wrapping the selector with square brackets ([ ]).

```
[data-type='primary'] {
  color: red;
}

<div data-type="primary"></div>

```

- Pseudo-elements differ from pseudo-classes because instead of responding to the platform state, they act as if they are inserting a new element with CSS. Pseudo-elements are also syntactically different from pseudo-classes, because instead of using a single colon (:), we use a double colon (::).

- A double colon (::) is what distinguishes a pseudo-element from a pseudo-class, but because this distinction wasn't present in older versions of CSS specs, browsers support a single colon for the original pseudo-elements, such as :before and :after to help with backwards compatibility with older browsers, like IE8.

```
.my-element::before {
  content: 'Prefix - ';
}
```

```
/* Your list will now either have red dots, or red numbers */
li::marker {
  color: red;
}
```

- A combinator is what sits between two selectors. For example, if the selector was p > strong, the combinator is the > character. The selectors which use these combinators help you select items based on their position in the document.

- A subsequent combinator is very similar to a next sibling selector. Instead of a + character however, use a ~ character. How this differs is that an element just has to follow another element with the same parent, rather than being the next element with the same parent.

- A child combinator (also known as direct descendant) allows you more control over the recursion that comes with combinator selectors. By using the > character, you limit the combinator selector to apply only to direct children.

- Compound selectors example:

```
a.my-class {
  color: red;
}
```

## The cascade

CSS stands for Cascading Stylesheets. The cascade is the algorithm for solving conflicts where multiple CSS rules apply to an HTML element.

- An inline style attribute with CSS declared in it will override all other CSS, regardless of its position, unless a declaration has !important defined.

- Position also applies in the order of your CSS rule. In this example, the element will have a purple background because background: purple was declared last. Because the green background was declared before the purple background, it is now ignored by the browser.

```
.my-element {
  background: green;
  background: purple;
}
```

- Being able to specify two values for the same property can be a simple way to create fallbacks for browsers that do not support a particular value. In this next example, font-size is declared twice. If clamp() is supported in the browser, then the previous font-size declaration will be discarded. If clamp() isn't supported by the browser, the initial declaration will be honored, and the font-size will be 1.5rem

```
.my-element {
  font-size: 1.5rem;
  font-size: clamp(1.5rem, calc(1rem + 3vw), 2rem);
}
```

- This approach of declaring the same property twice works because browsers ignore values they don't understand. Unlike some other programming languages, CSS will not throw an error or break your program when it detects a line it cannot parse—the value it cannot parse is invalid and therefore ignored. The browser then continues to process the rest of the CSS without breaking stuff it already understands.

- Specificity is an algorithm which determines which CSS selector is the most specific, using a weighting or scoring system to make those calculations. By making a rule more specific, you can cause it to be applied even if some other CSS that matches the selector appears later in the CSS.

```
<h1 class="my-element">Heading</h1>
```

- An id makes the CSS even more specific, so styles applied to an ID will override those applied many other ways. This is one reason why it is generally not a good idea to attach styles to an id. It can make it difficult to overwrite that style with something else.

The order of specificity of these origins, from least specific, to most specific is as follows:

1. User agent base styles. These are the styles that your browser applies to HTML elements by default.
2. Local user styles. These can come from the operating system level, such as a base font size, or a preference of reduced motion. They can also come from browser extensions, such as a browser extension that allows a user to write their own custom CSS for a webpage.
3. Authored CSS. The CSS that you author.
4. Authored !important. Any !important that you add to your authored declarations.
5. Local user styles !important. Any !important that come from the operating system level, or browser extension level CSS.
6. User agent !important. Any !important that are defined in the default CSS, provided by the browser.

## Specificity

- Each selector rule gets a scoring.

- A universal selector (\*) has no specificity and gets 0 points.

- An element (type) or pseudo-element selector gets 1 point of specificity .

- A class, pseudo-class or attribute selector gets 10 points of specificity.

- An ID selector gets 100 points of specificity, as long as you use an ID selector (#myID) and not an attribute selector ([id="myID"]).

- CSS applied directly to the style attribute of the HTML element, gets a specificity score of 1,000 points. This means that in order to override it in CSS, you have to write an extremely specific selector.

- Lastly, an !important at the end of a CSS value gets a specificity score of 10,000 points. This is the highest specificity that one individual item can get.

## Inheritance

- The color property is inheritable by other elements.

- Properties that can be inherited cascade downwards, and child elements will get a computed value which represents its parent's value. This means that if a parent has font-weight set to bold all child elements will be bold, unless their font-weight is set to a different value, or the user agent stylesheet has a value for font-weight for that element.

Example

- You can make any property inherit its parent's computed value with the inherit keyword. A useful way to use this keyword is to create exceptions.

```
strong {
  font-weight: 900;
}
```

- This CSS snippet sets all <strong> elements to have a font-weight of 900, instead of the default bold value, which would be the equivalent of font-weight: 700.

```
.my-component {
  font-weight: 500;
}
```

- The .my-component class sets font-weight to 500 instead. To make the <strong> elements inside .my-component also font-weight: 500 add:

```
.my-component strong {
  font-weight: inherit;
}
```

- Now, the <strong> elements inside .my-component will have a font-weight of 500.

- You could explicitly set this value, but if you use inherit and the CSS of .my-component changes in the future, you can guarantee that your <strong> will automatically stay up to date with it.

### The initial Keyword

- The initial keyword sets a property back to that initial, default value.

- This snippet will remove the bold weight from all <strong> elements inside an <aside> element and instead, make them normal weight, which is the initial value.

```
// CSS

aside strong {
  font-weight: initial;
}


/* presentational styles */
article > * + * {
  margin-top: 1em;
}

//HTML

<article>
  <p><strong>I am a strong element that is bold because that is the inherited user agent style.</strong></p>
  <aside>
    <p><strong>I am a strong element but my font weight is normal because it is in an aside element.</strong></p>
  </aside>
</article>
```

### The unset Keyword

- If a property is inheritable, the unset keyword will be the same as inherit. If the property is not inheritable, the unset keyword is equal to initial.

- For example, color is inheritable, but margin isn't, so you can write this:

```
/* Global color styles for paragraph in authored CSS */
p {
  margin-top: 2em;
  color: goldenrod;
}

/* The p needs to be reset in asides, so you can use unset */
aside p {
  margin: unset;
  color: unset;
}
```

- If you change the aside p rule to all: unset instead, it doesn't matter what global styles are applied to p in the future, they will always be unset.

```
aside p {
	margin: unset;
	color: unset;
	all: unset;
}
```

## Sizing Units

- For this case, you can use a ch unit, which is equal to the width of a "0" character in the rendered font at its computed size. This unit allows you to limit the width of text with a unit that's designed to size text, which in turn, allows predictable control regardless of the size of that text. The ch unit is one of a handful of units that are helpful for specific contexts like this example.

- It's a good idea to use a unitless value for line-height, rather than specifying a unit. As you learned in the inheritance module, font-size can be inherited. Defining a unitless line-height keeps the line-height relative to the font size. This provides a better experience than, say, line-height: 15px, which will not change and might look strange with certain font sizes.

- When using a percentage in CSS you need to know how the percentage is calculated. For example,width is calculated as a percentage of the width of the parent element.

```
div {
  width: 300px;
  height: 100px;
}

div p {
  width: 50%; /* calculated: 150px */
}
```

- The transform property allows you alter an element's appearance and position by rotating, skewing, scaling and translating it. This can be done in a 2D and 3D space.

- If you attach a unit to a number, it becomes a dimension. For example, 1rem is a dimension. In this context, the unit that is attached to a number is referred to in specifications as a dimension token. Lengths are dimensions that refer to distance and they can either be absolute or relative.

- By sizing text with relative units like em or rem, rather than an absolute unit, like px, the size of your text can respond to user preferences. This can include the system font size or parent element's font size, such as the <body>. The base size of the em is the element's parent and the base size of the rem is the base font size of the document.

- If you don't define a font-size on your html element, this user-preferred system font size will be honoured if you use relative lengths, such as em and rem. If you use px units for sizing text, this preference will be ignored.

### Absolute Lengths

- All absolute lengths resolve against the same base, making them predictable wherever they're used in your CSS.

- Eg. cm, mm, in, px, pt (points)

### Relative Lengths

- A relative length is calculated against a base value, much like a percentage. The difference between these and percentages is that you can contextually size elements. This means that CSS has units such as ch that use the text size as a basis, and vw which is based on the width of the viewport (your browser window). Relative lengths are particularly useful on the web due to its responsive nature.

- Eg. em, ex, ch, rem

- em: Relative to the font size, i.e. 1.5em will be 50% larger than the base computed font size of its parent. (Historically, the height of the capital letter "M").

- rem: Font size of the root element (default is 16px).

### Viewport-relative Units

- vw: 1% of viewport's width. People use this unit to do cool font tricks, like resizing a header font based on the width of the page so as the user resizes, the font will also resize.

- vh: 1% of viewport's height. You can use this to arrange items in a UI, if you have a footer toolbar for example.

- vmin: 1% of the viewport's smaller dimension.

- vmax: 1% of the viewport's larger dimension.

## Layout

- The display property does two things. The first thing it does is determine if the box it is applied to acts as inline or block.

- Inline elements behave like words in a sentence. They sit next to each other in the inline direction. Elements such as <span> and <strong>, which are typically used to style pieces of text within containing elements like a <p> (paragraph), are inline by default. They also preserve surrounding whitespace.

```
.my-element {
  display: inline;
}
```

- Block elements don't sit alongside each other. They create a new line for themselves. Unless changed by other CSS code, a block element will expand to the size of the inline dimension, therefore spanning the full width in a horizontal writing mode. The margin on all sides of a block element will be respected.

```
.my-element {
	display: block;
}
```

- Setting the display property to display: flex makes the box a block-level box, and also converts its children to flex items. This enables the flex properties that control alignment, ordering and flow.

```
.my-element {
	display: flex;
}
```

- Grid is similar in a lot of ways to flexbox, but it is designed to control multi-axis layouts instead of single-axis layouts (vertical or horizontal space).

### Flow Layout

- If not using grid or flexbox, your elements display in normal flow. There are a number of layout methods that you can use to adjust the behavior and position of items when in normal flow.

- Using `inline-block` gives you a box that has some of the characteristics of a block-level element, but still flows inline with the text.

- The float property instructs an element to "float" to the direction specified. The image in this example is instructed to float left, which then allows sibling elements to "wrap" around it. You can instruct an element to float left, right or inherit.

- When you use float, keep in mind that any elements following the floated element may have their layout adjusted. To prevent this, you can clear the float, either by using clear: both on an element that follows your floated element or with display: flow-root on the parent of your floated elements.

### Positioning

- The position property changes how an element behaves in the normal flow of the document, and how it relates to other elements. The available options are relative, absolute, fixed and sticky with the default value being static.

## Flexbox

- The key to understanding flexbox is to understand the concept of a main axis and a cross axis. The main axis is the one set by your flex-direction property. If that is row your main axis is along the row, if it is column your main axis is along the column.

- Flex items move as a group on the main axis. Remember: we've got a bunch of things and we are trying to get the best layout for them as a group.

- The cross axis runs in the other direction to the main axis, so if flex-direction is row the cross axis runs along the column.

- row: the items lay out as a row.

- row-reverse: the items lay out as a row from the end of the flex container.

- column: the items lay out as a column.

- column-reverse : the items lay out as a column from the end of the flex container.

- You should be cautious when using any properties that reorder the visual display away from how things are ordered in the HTML document, as it can negatively impact accessibility. The row-reverse and column-reverse values are a good example of this. The reordering only happens for the visual order, not the logical order. This is important to understand as the logical order is the order that a screen reader will read out the content, and anyone navigating using the keyboard will follow.

- The initial value of the flex-wrap property is nowrap. This means that if there is not enough space in the container the items will overflow.

- You can set the flex-direction and flex-wrap properties using the shorthand flex-flow. For example, to set flex-direction to column and allow items to wrap:

```
.container {
  display: flex;
  flex-flow: column wrap;
}
```

- flex-grow: 0: items do not grow.

- flex-shrink: 1: items can shrink smaller than their flex-basis.

- flex-basis: auto: items have a base size of auto.

- This can be represented by a keyword value of flex: initial. The flex shorthand property, or the longhands of flex-grow, flex-shrink and flex-basis are applied to the children of the flex container.

- justify-content: space distribution on the main axis.

- align-content: space distribution on the cross axis.

- place-content: a shorthand for setting both of the above properties.

- align-self: aligns a single item on the cross axis

- align-items: aligns all of the items as a group on the cross axis

- With the HTML used earlier, the flex items laid out as a row, there is space on the main axis. The items are not big enough to completely fill the flex container. The items line up at the start of the flex container because the initial value of justify-content is flex-start. The items line up at the start and any extra space is at the end. Add the justify-content property to the flex container, give it a value of flex-end, and the items line up at the end of the container and the spare space is placed at the start.

```
.container {
  display: flex;
  justify-content: flex-end;
}
```

-For the justify-content property to do anything you have to have spare space in your container on the main axis. If your items fill the axis then there is no space to share out so the property won't do anything.

## Grid

- CSS Grid Layout provides a two dimensional layout system, controlling layout in rows and columns. In this module discover everything grid has to offer.

- Grid is exceptionally useful at combining the control that extrinsic sizing provides with the flexibility of intrinsic sizing, which makes it ideal for this sort of layout. This is because grid is a layout method designed for two-dimensional content. That is, laying things out in rows and columns at the same time.

- A grid can be defined with rows and columns. You can choose how to size these row and column tracks or they can react to the size of the content.

- Direct children of the grid container will be automatically placed onto this grid.

- Or, you can place the items in the precise location that you want.

- Lines and areas on the grid can be named to make placement easier.

- Spare space in the grid container can be distributed between the tracks.

- Grid items can be aligned within their area.

- A grid is made up of lines, which run horizontally and vertically. If your grid has four columns, it will have five column lines including the one after the last column.

- A track is the space between two grid lines. A row track is between two row lines and a column track between two column lines. When we create our grid we create these tracks by assigning a size to them.

- A grid cell is the smallest space on a grid defined by the intersection of row and column tracks. It's just like a table cell or a cell in a spreadsheet. If you define a grid and don't place any of the items they will automatically be laid out one item into each defined grid cell.

- Several grid cells together. Grid areas are created by causing an item to span over multiple tracks.

- A gutter or alley between tracks. For sizing purposes these act like a regular track. You can't place content into a gap but you can span grid items across it.

- The min-content keyword will make a track as small as it can be without the track content overflowing. Changing the example grid layout to have three column tracks all at min-content size will mean they become as narrow as the longest word in the track.

- The max-content keyword has the opposite effect. The track will become as wide enough for all of the content to display in one long unbroken string. This might cause overflows as the string will not wrap.

- The fit-content() function acts like max-content at first. However, once the track reaches the size that you pass into the function, the content starts to wrap. So fit-content(10em) will create a track that is less than 10em, if the max-content size is less than 10em, but never larger than 10em.'

- The fr unit works in a similar way to using flex: auto in flexbox. It distributes space after the items have been laid out. Therefore to have three columns which all get the same share of available space:

```
.container {
    display: grid;
    grid-template-columns: repeat(12, minmax(0,1fr));
}
```

```
.container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(2, 200px 100px);
}

.item {
    grid-column-start: 1; /* start at column line 1 */
    grid-column-end: 4; /* end at column line 4 */
    grid-row-start: 2; /*start at row line 2 */
    grid-row-end: 4; /* end at row line 4 */
}
```

```
header {
    grid-area: header;
}

.sidebar {
    grid-area: sidebar;
}

.content {
    grid-area: content;
}

footer {
    grid-area: footer;
}
```

```
.container {
    display: grid;
    grid-template-columns: repeat(4,1fr);
    grid-template-areas:
        "header header header header"
        "sidebar content content content"
        "sidebar footer footer footer";
}
```

## Logical Properties

- Logical, flow relative properties and values are linked to the flow of text, rather than the physical shape of the screen.

- Block flow is the direction in which content blocks are placed. For example, if there are two paragraphs, the block flow is where the second paragraph will go. In an English document, the block flow is top-to-bottom. Think of this in the context of paragraphs of text following each other, top-to-bottom.

- The inline flow is how text flows in a sentence. In an English document the inline flow is left to right. If you were to change the document language of your webpage to Arabic (<html lang="ar">), then the inline flow would be right-to-left.

- The flow-relative equivalents are max-inline-size and max-block-size. You can also use min-block-size and min-inline-size instead of min-height and min-width.

- Logical properties for margin, padding and inset make positioning elements, and determining how these elements interact with each other across writing modes easier and more efficient. The margin and padding related properties are still direct mappings to directions, but the key difference is that when a writing mode changes, they change with it.

- The inset-block property isn't the only shorthand option with logical properties. This rule can be further condensed, using shorthand versions of the margin and padding properties.

```
p svg {
  width: 1.2em;
  height: 1.2em;
  margin-inline-end: 0.5em; //changed
  flex
```

## Spacing

- If you use a <br> element, it will create a line-break, just like if you were to press your enter key in a word processor.

- The <hr> creates a horizontal line with space either-side, known as margin.

- If you want to add space to the outside of an element, you use the margin property.

- If you make an element absolutely positioned, using position: absolute, the margin will no longer collapse. The margin also won't collapse if you use the float property, too.

- If you have an element with no margin between two elements with block margin, the margin won't collapse either, because the two elements with block margin are no longer adjacent siblings: they are just siblings.

- An element with position: relative will maintain its place in the document flow, even when you set these values. They will be relative to your element's position too.

- An element with position: absolute will base the directional values from the relative parent's position.

- An element with position: fixed will base the directional values on the viewport.

- An element with position: sticky will only apply the directional values when it is in its docked/stuck state.

## Pseudo-elements

- A pseudo-element is like adding or targeting an extra element without having to add more HTML

```
p::first-letter {
  color: blue;
  float: left;
  font-size: 2.6em;
  font-weight: bold;
  line-height: 1;
  margin-inline-end: 0.2rem;
}
```

- Both the ::before and ::after pseudo-elements create a child element inside an element only if you define a content property.

```
.my-element::before {
	content: "";
}

.my-element::after {
	content: "";
}
```

- input[type="checkbox"] is an exception. It is allowed to have pseudo-element children.

### ::first-letter / ::first-line

- color

- background properties

- border

- float

- font

- text properties

- ::first-line applied has a display value of block, inline-block, list-item, table-caption or table-cell.

- If you have an element that is presented in full screen mode, such as a <dialog> or a <video>, you can style the backdrop—the space between the element and the rest of the page—with the ::backdrop pseudo-element:

- The ::marker pseudo-element lets you style the bullet or number for a list item or the arrow of a <summary> element.

- The ::selection pseudo-element allows you to style how selected text looks.

- You can add a helper hint to form elements, such as <input> with a placeholder attribute. The ::placeholder pseudo-element allows you to style that text.

- The ::placeholder only supports a subset of CSS rules:
  - color
  - background properties
  - font properties
  - text properties

## Pseudo-classes

- Pseudo-classes let you apply CSS based on state changes

- Say you've got an email sign up form, and you want the email form field to have a red border if it contains an invalid email address. How do you do that? You can use an :invalid CSS pseudo-class, which is one of many browser-provided pseudo-classes.

- :hover, :active,:focus, :focus-within, :focus-visible, :target, :link, :visited, :disabled, :enabled, :checked, :indeterminate, :valid, :invalid, :empty, :not

- first-child, last-child, only-child, nth-child, nth-of-type

## Shadows

- The box-shadow property is for adding shadows to the box of an HTML element. It works on block elements and inline elements.

```
.my-element {
	box-shadow: 5px 5px 20px 5px #000;
}
```

- Horizontal offset: a positive number pushes it out from the left and a negative number will push it out from the right.

- Vertical offset: a positive number pushes it down from the top, and a negative number will push it up from the bottom.

- Blur radius: a larger number produces a more blurred shadow, whereas a small number produces a sharper shadow.

- Spread radius (optional): a larger number increases the size of the shadow and a smaller number decreases it, making it the same size as the blur radius if it's set to 0.

- Color: Any valid color value. If this isn't defined, the computed text color will be used.

```
/* Outer shadow */
.my-element {
	box-shadow: 5px 5px 20px 5px #000;
}

/* Inner shadow */
.my-element {
	box-shadow: inset 5px 5px 20px 5px #000;
}
```

- You can add as many shadows as you like with text-shadow, just as with box-shadow. Add a comma separated collection of value sets, and you can create some really cool text effects, such as 3D text.

```
// CSS
.my-element {
  text-shadow: 1px 1px 0px white,
    2px 2px 0px firebrick;
  color: darkslategray;
}

body {
  background: #f3f3f3;
}

/* Presentational styles */
.my-element {
  --flow-space: 2rem;
  font-size: 3em;
  font-weight: 900;
  font-family: 'Roboto Condensed', sans-serif;
  text-transform: uppercase;
  line-height: 1.1;
  max-width: 40ch;
}


// HTML
<h1 class="my-element">
  Multiple text shadows are cool
</h1>
```

- To achieve a drop shadow that follows any potential curves of an image, use the CSS drop-shadow filter. This shadow is applied to an alpha mask which makes it very useful for adding a shadow to a cutout image, as in the case in the intro of this module.

```
.my-image {
  filter: drop-shadow(0px 0px 10px rgba(0 0 0 / 30%))
}
```

- The drop-shadow filter has the same values as box-shadow but the inset keyword and spread value are not allowed. You can add as many shadows as you like, by adding multiple instances of drop-shadow values to the filter property. Each shadow will use the last shadow as a positioning reference point.

## Focus

- Focus styles assist people who use a device such as a keyboard or a switch control to navigate and interact with a website. If an element receives focus and there is no visual indication, a user may lose track of what is in focus. This can create navigation issues and result in unwanted behaviour if, say, the wrong link is followed. You can read more on focus and how to manage it in this guide.

- You can typically navigate a website's focusable elements using the tab key to move forward on the page, and shift + tab to move backward.

- There is also a HTML attribute called tabindex which allows you to change the tabbing index—which is order in which elements are focused—every time someone presses their tab key, or focus is shifted with a hash change in the URL or by a JavaScript event. If tabindex on a HTML element is set to 0, it can receive focus via the tab key and it will honour the global tab index, which is defined by the document source order.

- Avoid using outline: none on an element that can receive keyboard focus

- Avoid replacing outline styles with box-shadow as they don't show up in Windows High Contrast Mode

- Only set a positive value for tabindex on an HTML element if you absolutely have to

- Make sure the focus state is very clear vs the default state

## Z-index and Stacking Context

- The z-index property explicitly sets a layer order for HTML based on the 3D space of the browser—the Z axis. This is the axis which shows which layers are closer to and further from you. The vertical axis on the web is the Y axis and the horizontal axis is the X axis.

Example

```
// HTML
<main>
  <div class="wrapper">
    <article class="flow">
      <h1>Default <code>z-index</code> behaviour</h1>
      <figure class="callout">
        <p>
          If no value for <code>z-index</code> is set, the browser will use the
          document source order to dictate <code>z-index</code> instead.
        </p>
        <p>
          This demo has 3 empty <code>&lt;div&gt;</code> elements, with negative
          margin, making them overlap. The later elements sit on top of the earlier
          elements.
        </p>
      </figure>
      <div class="demo">
        <div>1</div>
        <div>2</div>
        <div>3</div>
      </div>
    </article>
  </div>
</main>

// CSS
.demo > * {
  width: 250px;
  height: 200px;
}

.demo > * + * {
  margin-top: -150px;
  opacity: 0.8;
  box-shadow: 0 -1px 10px rgba(0 0 0 / 60%);
}

.demo > :first-child {
  background: aliceblue;
  border: 2px solid lightblue;
}

.demo > :nth-child(2) {
  background: pink;
  border: 2px solid hotpink;
}

.demo > :last-child {
  background: wheat;
  border: 2px solid gold;
}
```

- To set an element behind another element, add a negative value for z-index.

Example

```
// HTML
<div class="my-element">
  <div class="child">I am on top of my parent</div>
</div>

// CSS
.my-element {
  position: relative;
  z-index: 0;

	background: rgb(232 240 254 / 0.5);
  border: 1px solid lightblue;
}

.my-element .child {
	position: relative;
	z-index: -1;

  background: pink;
  border: 1px solid hotpink;
  padding: 1rem;
  width: 275px;
}


/* Decorative styles */
.my-element {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 250px;
  height: 250px;
}

.child {
  font-weight: bold;
}
```

- Because .my-element now has a position value that's not static and a z-index value that's not auto, it has created a new stacking context. This means that even if you set .child to have a z-index of -999, it would still not sit behind .my-parent.

- A stacking context is a group of elements that have a common parent and move up and down the z axis together.

- You don't need to apply z-index and position to create a new stacking context. You can create a new stacking context by adding a value for properties which create a new composite layer such as opacity, will-change and transform.

- You can also create a stacking context by adding a filter and setting `backface-visibility: hidden`.

## Functions

- Functional selectors example:

```
.post :is(h1, h2, h3) {
	line-height: 1.2;
}
```

- The var() function takes one required argument: the custom property that you are trying to return as a value. In the above snippet, the var() function has --base-color passed as an argument. If --base-color is defined, then var() will return the value.

Example

```
.my-element {
	background: var(--base-color, hotpink);
}
```

- The calc() function takes a single mathematical expression as its parameter. This mathematical expression can be a mix of types, such as length, number, angle and frequency. Units can be mixed too.

Examples

```
.my-element {
	width: calc(100% - 2rem);
}
```

```
:root {
  --root-height: 5rem;
}

.my-element {
  width: calc(calc(10% + 2rem) * 2);
  height: calc(var(--root-height) * 3);
}
```

- The min() function returns the smallest computed value of the one or more passed arguments. The max() function does the opposite: get the largest value of the one or more passed arguments.

Example

```
.my-element {
  width: min(20vw, 30rem);
  height: max(20vh, 20rem);
}
```

- The clamp() function takes three arguments: the minimum size, the ideal size and the maximum.

Example

```
h1 {
  font-size: clamp(2rem, 1rem + 3vw, 3rem);
}
```

- In this example, the font-size will be fluid based on the width of the viewport. The vw unit is added to a rem unit to assist with screen zooming, because regardless of zoom level a vw unit will be the same size. Multiplying by a rem unit—based on the root font size— provides the clamp() function with a relative calculation point.

## Gradients

- The linear-gradient() function generates an image of two or more colors, progressively. It takes multiple arguments, but in its simplest configuration, you can pass some colors like this and it will automatically split them evenly, while blending them.

```
.my-element {
	background: linear-gradient(black, white);
}
```

- To create a gradient that radiates in a circular fashion, the radial-gradient() function steps in to help. It's similar to linear-gradient(), but instead of specifying an angle, you optionally specify a position and ending shape. If you just specify colors, the radial-gradient() will auto-select the position as center and select either a circle or ellipse, depending on the size of the box.

```
.my-element {
	background: radial-gradient(white, black);
}
```

- A conic gradient has a center point in your box and starts from the top (by default), and goes around in a 360 degree circle.

```
.my-element {
	background: conic-gradient(white, black);
}
```

### Mixing Gradients

```
// CSS
.my-element {
	background: linear-gradient(-45deg, blue -30%, transparent 80%), linear-gradient(45deg, darkred 20%, crimson, darkorange 60%, gold, bisque);
}

/* Presentational styles */
.my-element {
  width: 250px;
  height: 250px;
}

// HTML
<div class="my-element"></div>
```

## Animations

```
@keyframes my-animation {
	from {
		transform: translateY(20px);
	}
	to {
		transform: translateY(0px);
	}
}
```

Full example

```
// CSS
.pulser {
  width: 30px;
  height: 30px;
  background: rebeccapurple;
  border-radius: 50%;
  position: relative;
}

.pulser::after {
  animation: pulse 1000ms cubic-bezier(0.9, 0.7, 0.5, 0.9) infinite;
}

@keyframes pulse {
  0% {
    opacity: 0;
  }
  50% {
    transform: scale(1.4);
    opacity: 0.4;
  }
}

.pulser::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: blueviolet;
  border-radius: 50%;
  z-index: -1;
}

/* Decorative styles */
body {
  display: grid;
  place-items: center;
}

// HTML
<div class="pulser"></div>
```

- The animation-duration property defines how long the @keyframes timeline should be. It should be a time value. It defaults to 0 seconds, which means the animation still runs, but it'll be too quick for you to see. You can't add negative time values.

```
.my-element {
	animation-duration: 10s;
}
```

- To help recreate natural motion in animation, you can use timing functions that calculate the speed of an animation at each point. Calculated values are often curved, making the animation run at variable speeds over the course of animation-duration, and if a value is calculated beyond that of the value defined in @keyframes, make the element appear to bounce.

```
.my-element {
	animation-timing-function: ease-in-out;
}
```

- The animation-iteration-count property defines how many times the @keyframes timeline should run. By default, this is 1, which means that when the animation reaches the end of your timeline, it will stop at the end. The number can't be a negative number.

```
.my-element {
	animation-iteration-count: 10;
}
```

- You can use the infinite keyword which will loop your animation

- normal: the default value, which is forwards.

- reverse: runs backwards over your timeline.

- alternate: for each animation iteration, the timeline will run forwards or backwards in sequence.

- alternate-reverse: the reverse of alternate.

- The animation-play-state property allows you to play and pause the animation. The default value is running and if you set it to paused, it will pause the animation.

```
.my-element:hover {
	animation-play-state: paused;
}
```

- The animation-fill-mode property defines which values in your @keyframes timeline persist before the animation starts or after it ends. The default value is none which means when the animation is complete, the values in your timeline are discarded. Other options are:

- forwards: The last keyframe will persist, based on the animation direction.

- backwards: The first keyframe will persist, based on the animation direction.

- both: follows the rules for both forwards and backwards.

- Instead of defining all the properties separately, you can define them in an animation shorthand, which lets you define the animation properties in the following order:

  1. animation-name

  2. animation-duration

  3. animation-timing-function

  4. animation-delay

  5. animation-iteration-count

  6. animation-direction

  7. animation-fill-mode

  8. animation-play-state

```
.my-element {
	animation: my-animation 10s ease-in-out 1s infinite forwards forwards running;
}
```
