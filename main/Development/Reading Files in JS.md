Tags: #js #frontend 
Created: 2022-08-20 15:08
References: 

# Reading Files in JS
In order to read contents of a file in [[JavaScript]] you can use an _input_ element in [[HTML]] (this example is made using react but the principle is the same for anything)

```html
<input type="file" accept=".json" onChange={handleFileSelected} />
```

onChange event gives you a FileList in _.target.files_ with this you can create a _FileReader_ and add an event listener on the _load_ event

After adding event listener call any of the read methods

```js
const handleFileSelected = (e) => {
const file = e.target.files[0];
const reader = new FileReader();
reader.addEventListener("load", event => {
	const resultText = event.target?.result;
		setData(JSON.parse(resultText));
	});
	reader.readAsText(file);
};
```

(example from spektrum visualizer App.tsx)