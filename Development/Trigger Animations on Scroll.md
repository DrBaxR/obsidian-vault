---
tags:
 - frontend
---

In order to trigger an animation when an [[HTML]] element gets into the view from scrolling, you can just add a [[CSS]] class to the element that you want to animate. For example, let's say you have this class:

```css
.animation-class {
	animation: wipe-enter 1s 1;
}
```

If you programatically add this class to an element (by using a little [[JavaScript]]), the _wipe-enter_ animation will play. Now that we know how to play an animation, all that's left is detecting when the element gets scrolled to.

This can be done with the [[Intersection Observer API]], which keeps track when an element intersects with another. The way this works is you create an observer, which tracks some elements (which you specify). This observer fires the callback each time the intersection state of any of the tracked elements changes. Here is an example (that triggers the animation once per page load):

```javascript
const observer = new IntersectionObserver(entries => {
	entries.forEach(entry => {
		if (entry.isIntersecting) {
			entry.target.classList.add('animation-class');
		}
	})
});

observer.observe(document.querySelector('#testAnimation'));
```

If the element that is getting animated changes size or position during the animation it can be tricky for the browser to keep track whether it is intersecting or not (which can lead to bugs) so it it a good practice to wrap the element that changes size/position/whatever in another element that is static:

```html
<div class="square-wrapper">
	<div class="square"></div>
</div>
```

### Resources
[https://coolcssanimation.com/how-to-trigger-a-css-animation-on-scroll/](https://coolcssanimation.com/how-to-trigger-a-css-animation-on-scroll/)
[https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)