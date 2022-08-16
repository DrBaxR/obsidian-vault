---
tags: #css 
---

# Pseudo-element
A [[CSS]] pseudo-element is a keyword that you can add to a selector that selects only a part of the element that you would normally select.

An example of a pseudo-element is `::first-line`, which selects the first line of a paragraph

```css
/* first line of all paragraphs are red */
p::first-line {
	color: red;
}
```

**Rule**: Double colons (`::`) should be used instead of single colons (`:`). This is a convention to distinguish between [[Pseudo-element]]s and [[Pseudo-class]]es.

### Resources
https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements