---
tags: #css 
---
# before & after (pseudo-elements)
`::before` is a [[CSS]] [[Pseudo-element]] that is the first child of the selected element. It is often used to add cosmeti stuff to the element using its `content` property.

`::after` is basically the same thing as `::before`, but it represents the last child of the element that was selected.

Both of these are inline by default and **require** to have a `content` property value in order to be shown.
```css
/* does nothing */
div::before {
	background: red;
}

/* does something */
div::before {
	content: '';
	background: red;
}
```