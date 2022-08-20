---
tags:
 - css
---

This is a 21 day course made by Kevin Powell.

## Day 1
Out of the box, websites are responsive. The more you want to fight against the natural flow of things, the more out-of-the-box responsiveness you are going to lose (which you are going to have to implement yourself). You run into an issue => YOU did something to make that happen.

Is is generally a bad idea to set the height of things. If you need more background let's say you can give it more padding.

**em** is relative to the font size of the thing that you are styling. For example, for a container that has a font-size of 16px, 5em = 5 * 16px.


## Day 2
**em** is always relative to the font size of the parent. ems are compounding: if you have 3 nested div elements - a, b, c -, and you explicitly set their font sized to _2em_, then (if the parent of _a_ has a font size of 16px) _a_ will have a font size of 32px (2 * 16px), _b_ will have a font size of 64px (2 * 32px) and _c_ will have a font size of 128px (2 * 64px).

_Note:_ For em, if you use it for anything else that is not font-size, it is going to be relative to the font-size of the _current element_, not to the font size of the parent of the current element! This can be a good thing - if you change the font size of an element, then the margin/padding/whatever automatically adjusts, keeping the styling consistent.

**rem** was invented to fix the compounding problem _em_ has. It looks at the _root_'s (the _html_ element of the page) font size and it is relative to that.

Use **em** when you want _adaptability_ and **rem** when you want _consistency_. The cool thing when using em and rem units for everything is that when doing media queries, you can only change a single property (the font-size) and everything automatically adapts.

## Day 3
Generally, it is a good design choice to limit the width of things. It gets really annoying for the user to have to actually turn their head when for example reading an article. For this reason, you should use the **max-width** property.

Another thing is that setting the **min-width** is generally a bad idea. Of course, there are certain cases when it is useful, but generally it _fights against responsiveness_ (it's pretty much like setting the width of something to a fixed value).

## Day 7
Instead of using fixed size padding for left-right white space, it may be a good idea to use _margin: auto_ and _width: x%_. The reason why this might be a better approach is because having the same fixed size white space to the left and to the right can become a bit too much/too little for smaller/bigger device screens.

_max-width_ is where you are allowed to use fixed values. On this day, he also talked about [[BEM]].

He likes using _rem_ for font sizes.

## Day 8
**CSS combinator**: `[selector]+[selector]`. This applies style if there are specified elements adjacent to each other (the element to which it applies the style is the last one). For the snippet below, the style gets applied to columns 2 and 3, but not 1, since it does not have a _col_ adjacent in before it.

```html
<div class="col">Column 1</div>
<div class="col">Column 2</div>
<div class="col">Column 3</div>
```

```css
.col + .col {
	background-color: green;
}
```

## Day 9
_Passing thought:_ Uneori chiar daca ai impresia ca unele 'feature'-uri (let's call them that) seamana cu niste chestii care deja sunt implementate este mai bine sa creezi o chestie noua decat sa modifici chestia existenta. Daca efortul de a modifica chestia existenta este mai mare decat crearea unei chestii noi, mai bine alegi a doua optiune.

If you have a _div_ that has its parent as a _display: flex_ and that div has an image for example in it, the image will not be considered a flex item and will not behave as you expect it to. In order to fix that, you can set the _max-width: 100%_ of the image.

## Day 15

**Desktop first** = your default style (the style that is applied without any media queries) is the one that gets applied for desktop

**Mobile first** = the same as desktop first, but defaults to mobile

Order of media queries matters (they can override each other). For example if you have some styles for _min-width: 800px_ and after it some styles for _min-width: 600px_, the 600px styles would never get applied.

## Day 16
Generally, for simple sites (you have a single layout) it is better to pick the breakpoints as you scale the page and the content starts to look bad. For large websites, on the other side...

Breakpoints are: _600px, 900px, 1200px, and 1800px if you plan on giving the giant-monitor people something special._

## Day 17
**Viewport meta tag** is needed to assure media queries work on mobile!

### Resources
[https://courses.kevinpowell.co/view/courses/conquering-responsive-layouts/233002-introduction/1007804-intro-why-the-course-is-formatted-in-this-way](https://courses.kevinpowell.co/view/courses/conquering-responsive-layouts/233002-introduction/1007804-intro-why-the-course-is-formatted-in-this-way)
[https://sparkbox.com/foundry/bem_by_example](https://sparkbox.com/foundry/bem_by_example)