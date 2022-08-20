Tags: #frontend #angular 
Created: 2022-08-20 16:08
References: 

# Structural Directive
## General description
Structural [[Directive]]s are things that you apply to an element in the HTML that modify the DOM. The out of the box ones are *ngIf, *ngFor and *ngSwitch.

To generate a directive, you can use the [[Angular CLI]]:

```sh
ng generate directive myDirective
ng g d myDirective
```

The * syntax is basically syntax sugar. Here's a comparison between the short syntax and the long one. Short first:

```html
<div *myDirective>
	This is my div
</div>
```

And the long one:

```html
<ng-template myDirective>
	<div>
		This is my div
	</div>
</ng-template>
```

You can use pretty much anything you can use in components inside directives: @Input(), [[Lifecycle Hooks]], [[Dependency Injection]] etc. In order to make the directive actually manipulate the [[DOM]], you need to inject a couple of things:

```ts
constructor(
	private viewContainerRef: ViewContainerRef,
	private templateRef: TemplateRef<any>,
) { }
```

**ViewContainerRef** - reference to where you can render stuff (the container of the thing that you applied the directive to)
**TemplateRef** - reference to the [[Template]] to which you applied the directive to

## Input tricks
If you want to use this syntax for giving an input to the directive:

```html
<div *appHideAfter="5000">
	<!-- ... -->
</div >
```

You need to have an input with the same name as the directive inside the directive:

```ts
@Directive({
	selector: '[appHideAfter]'
})
export class HideAfterDirective {
	@Input() appHideAfter = 0; // or @Input('appHideAfter') delay = 0
	// ...
}
```

You know how you can use **then** and **else** inside ngIf, like _*ngIf="condition; then trueTemplate; else falseTemplate"_? Well, you can do that with any keyword in your own templates. ***REALLY COOL!*** In order to do that, you only have to name some inputs like this: `${directiveName}${keyword}`. For example, the ngIf directive has two inputs that are called _ngIfThen_ and _ngIfElse._ Here's an example:

```ts
@Directive({
	selector: '[appHideAfter]'
})
export class HideAfterDirective {
	@Input('appHideAfter') delay = 0;
	@Input('appHideAfterThen') placeholder: TemplateRef<any> | null = null
	// ...
}
```

This lets you use the directive like this:

```html
<h1>Structural Directives</h1>
<section *appHideAfter="5000; then placeholderBanner">
	<h2>Temporary content</h2>
	<p>This layout should disappear in 5 seconds.</p>
</section>
<ng-template #placeholderBanner>
	<section>
		<h2>There used to be something here...</h2>
		<p>Not anymore tho 😉</p>
	</section>
</ng-template>
```

And of course, in case you wanted to use the directive like _*appHideAfter="5000; after placeholderBanner"_ , all you'd have to do is replace the **then** input with an **after** input (one that is called _appHideAfterAfter_).

## Resources
https://youtu.be/07CaGlbMPbw
https://betterprogramming.pub/mastering-in-angular-structural-directives-b089186136ef