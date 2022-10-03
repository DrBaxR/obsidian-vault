Tags: #frontend #angular 
Created: 2022-08-20 18:08
References: 

# Template Context
Contexts are basically inputs for ng-templates ([[Template]]s). You can create a template with a context (which is basically an object) and after that, you can set some properties like `let-[name]="[field]"` on the ng-template (where _name_ is the name of the variable and _field_ is the name of the context field). The field can be omitted and the variable will reference the _$implicit_ field of the context.

Example:
```html
<ng-container *ngTemplateOutlet="all; context: contextAll"></ng-container>
<ng-template #all
			 let-name
			 let-surname="contextSurname">
	<div>Implicit: {{ name }} Explicit: {{ surname }}</div>
</ng-template>
```

Where `contextAll` is:
```ts
contextAll = { $implicit: 'John', contextSurname: 'Doe' };
```
