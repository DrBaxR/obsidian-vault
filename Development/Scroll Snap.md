---
tags:
 - frontend
---

Scroll snap is a [[CSS]] module that, as its name suggests, can be used to set some snap points to which the view can snap to while scrolling.

The way you use it is you set **scroll-snap-type** on the container and **scroll-snap-align** on the elements inside the container.

### Container Properties
**_scroll-snap-type_**: mandatory or proximity. _mandatory_ means that the browser has to snap the a snap point when the user stops scrolling. _proximity_ means that the browser may snap to a snap point if it deems it appropriate.

**_scroll-padding_**: Snap points are normally exactly where the element is. You can use this property to "add some padding " to the place where the snap point is relative to the element.

### Element Properties
**_scroll-snap-align_**: start, center or end. Relative to the way you are scrolling (horizontally or vertically). Possible to have for example _start_ for vertical scrolling and _end_ for horizontal scrolling.

**_scroll-snap-stop_**: normal or always. Normally, scroll snapping snaps only when the user stops scrolling (they can miss elements). _scroll-snap-stop: always_ makes it so scroll stops on elements before user can continue scrolling. (at 2022-07-24 not supported by Firefox).

**Note:** Bug that happens often (in my experience) - the thing that scrolls is not the thing that has _scroll-snap-type_ set.

#### Resources
[https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type)
[https://drafts.csswg.org/css-scroll-snap/](https://drafts.csswg.org/css-scroll-snap/)
[https://css-tricks.com/practical-css-scroll-snapping/](https://css-tricks.com/practical-css-scroll-snapping/)