Tags: #frontend #angular 
Created: 2022-08-20 18:08
References:

# Structural Directive Context
[[Template Context]]s can be really useful when using [[Structural Directive]]s: If you want to use something like this (the part with **as**, where you can save an input as a value that you can use later in the template):

```html
<section *appHideAfter="5000 as time; then placeholderBanner"
		 class="banner">
	<h2>Temporary content</h2>
	<p>This layout should disappear in {{ time / 1000 }} seconds.</p>
</section>
```

When you use _createEmbeddedView_ you need to also provide a **context**. The context is basically an object that has a fields named like the inputs that you want to reference  with 'as' later. For example, for referencing the value given as the first input (the input that is called like the directive), your context needs to look a little like this:

```ts
class HideAfterContext {
	public appHideAfter = 1000;
}

@Directive({
	selector: '[appHideAfter]'
})
export class HideAfterDirective implements OnInit {
	@Input('appHideAfter') set delay(value: number) {
		this._delay = value;
		this.context.appHideAfter = value;
	};
	private _delay = 0;
	private context = new HideAfterContext();

	// ...

	ngOnInit(): void {
		this.viewContainerRef.createEmbeddedView(this.templateRef, this.context);
		// ...
	}
}
```

Note that the previous 'delay' property was replaced by a private field and the input was transformed to a setter that modified the context that is used when creating the embedded view.

If you want to use something from within the context by using the **let** keyword (something like the example below - the _let counterRef = counter_)

```html
<section *appHideAfter="5000 as time; then placeholderBanner; let counterRef = counter"
		 class="banner">
	<h2>Temporary content</h2>
	<p>This layout should disappear in {{ time / 1000 }} seconds.</p>
	<p>Time left: {{ counterRef }}</p>
</section>
```

You need to have something that is called **counter** inside your context. This will let you reference that value from outside the directive

```ts
class HideAfterContext {
	// ...
	public counter = 0;
}

@Directive({
	selector: '[appHideAfter]'
})
export class HideAfterDirective implements OnInit {
	@Input('appHideAfter') set delay(value: number | null) {
		// ...
		this.context.counter = this._delay / 1000;
	};
	
	// ...
	ngOnInit(): void {
		// ...
		setInterval(() => { this.context.counter-- }, 1000);
		// ...
	}
}
```

If you want to reference the time that it took for the thing to disappear in the template that gets rendered after the initial template is hidden, you need to also provide that context when creating the placeholder (in the directive):

```ts
// ...

@Directive({
	selector: '[appHideAfter]'
})
export class HideAfterDirective implements OnInit {
	// ...
	ngOnInit(): void {
		// ...
		setTimeout(() => {
			// ...
			this.viewContainerRef.createEmbeddedView(this.placeholder, this.context);
			// ...
		}, this._delay)
	}
}
```

Now that you provided the placeholder with a context, you can get the delay from the context in your [[HTML]] by using this syntax:

```html
<ng-template #placeholderBanner
			 let-hiddenAfter="appHideAfter">
	<!-- ... -->
	<p>The thing vanished in {{ hiddenAfter / 1000 }} seconds...</p>
</ng-template>
```
syntax is _let-<name_you_pick>="<context_variable_to_reference>"_

If you have a fields that is called _$implicit_ inside the context, then that HTML will not have to contain the part after '='. Context:

```ts
this.viewContainerRef.createEmbeddedView(this.placeholder, { $implicit: this.context.appHideAfter });
```

```html
<ng-template #placeholderBanner
			 let-hiddenAfter>
	<!-- ... -->
	<p>The thing vanished in {{ hiddenAfter / 1000 }} seconds...</p>
</ng-template>
```
This sequence of code and the previous one do the same thing.

## Resources
https://youtu.be/zpVVHI21TAo