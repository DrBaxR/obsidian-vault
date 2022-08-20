Tags: #angular #directives
Created: 2022-08-20 16:08
References: 

# Attribute Directives
A [[Directive]] that you apply as you would specify a [[HTML]] element's attribute. Create with [[CLI]] and inject _ElementRef_ to access reference to host [[DOM]] element of attribute.

```ts
@Directive({
	selector: '[appHighlight]'
})
export class HighlightDirective {
	constructor(
		private element: ElementRef
	) {
		const el = element.nativeElement as HTMLElement;
		el.style.background = 'yellow';
	}
}
```

In order to process user inputs on the element on which the directive is applied on, use _@HostListener()_ [[Decorator]].

```ts
@HostListener('mouseenter') onMouseEnter() {
	this.highlight('yellow');
}
```

For user inputs of the directive, _@Input()_ can be used. It the input has the same name as the directive, short syntax can be used.

```ts
@Directive({
	selector: '[appHighlight]'
})
export class HighlightDirective {
	@Input('appHighlight') color = '';
	
	// ...
}
```

```html
<span [appHighlight]="color">This is some text.</span>
```

Other inputs that are not named like the directive, you can apply as usual. Example for another string input:

```html
<p [appHighlight]="color"
   defaultColor="violet">
	This is some text.
</p>
```

## Resources
https://angular.io/guide/attribute-directives