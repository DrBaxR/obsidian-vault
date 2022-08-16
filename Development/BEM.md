---
tags:
 - css
---

**BEM (Block Element Modifier)** naming convention that is used in [[CSS]]. 

This one can make you avoid code duplication by following it while also making your css easier to change in the future. The way this works is you have **Blocks**, which are something like components; these are named however you want. Besides blocks, you also have **Elements**, children of blocks that are named like `[block_name]__[element-name]`. The final type of entities are **Modifiers**, which are modifications - or variations - of blocks and elements. the way you name them is `[modified-entity]--[modifier]`.

In general, the structure of an identifier is something like this: `[block]__[element]--[modifier]`.

```html
<button class="btn btn--secondary"></button>

<style>
	.btn {
		display: inline-block;
		color: blue;
	}
	
	.btn--secondary {
		color: green;
	}
</style>
```

Here's an example of what to do:
```html
<figure class="photo">
	<img class="photo__img" src="me.jpg">
	<figcaption class="photo__caption">Look at me!</figcaption>
</figure>

<style>
.photo { } /* Specificity of 10 */
.photo__img { } /* Specificity of 10 */
.photo__caption { } /* Specificity of 10 */
</style>
```

And an example of what **not** to do:
```html
<figure class="photo">
	<img src="me.jpg">
	<figcaption>Look at me!</figcaption>
</figure>

<style>
.photo { } /* Specificity of 10 */
.photo img { } /* Specificity of 11 */
.photo figcaption { } /* Specificity of 11 */
</style>
```

### Resources
[https://www.youtube.com/watch?v=SLjHSVwXYq4](https://www.youtube.com/watch?v=SLjHSVwXYq4)