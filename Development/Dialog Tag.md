---
tags:
 - html
 - frontend
---

The [[HTML]] dialog tag represents the element of a **dialog box** or some other interractive component, such as a dismissible alert, inspector or subwindow (can be used to make a modal).

```html
<dialog open>
	<p>Greetings, one and all!</p>
	<form method="dialog">
		<button>OK</button>
	</form>
</dialog>
```

You can style the background of the modal via the [[::backdrop]] [[CSS]] [[Pseudo-element]]. Also you can show/close the modal from [[JavaScript]] by using the _showModal()_ and respectively _close()_ methods.

```javascript
const modal = document.getElementById('modal');
// ...
modal.showModal();
// ...
modal.close();
```
