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
