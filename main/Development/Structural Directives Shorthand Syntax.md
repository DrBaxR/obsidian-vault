Tags: #frontend #angular 
Created: 2022-08-20 18:08
References: 

# Structural Directives Shorthand Syntax
The syntax that you are used to for the built-in [[Angular]] [[Structural Directive]]s is actually the shorthand notation.

**Simple ngFor:**
```html
<!-- shorthand -->
*ngFor="let item of [1,2,3]"

<!-- long -->
<ng-template ngFor
			 let-item
			 [ngForOf]="[1,2,3]">
```

**More complex ngFor:**
```html
<!-- shorthand -->
*ngFor="let item of [1,2,3] as items;
		trackBy: myTrack; index as i"
  
<!-- long -->
<ng-template ngFor
			 let-item
			 [ngForOf]="[1,2,3]"
			 let-items="ngForOf"
			 [ngForTrackBy]="myTrack"
			 let-i="index">
```

## Resources:
https://angular.io/guide/structural-directives