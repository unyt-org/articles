<!--
	{
		description: "Why we don't need React (But JSX is cool) - An introduction to the UIX framework",
		preview: "res/uix_banner.png",
		date: ~2023-04-26~,
		tag: "Developer",
		author: "Benedikt Strehle",
		authorRef: https://github.com/benStre
	};
-->


# Back to the roots - developing a web native framework

## The state of the web

The web platform has come far in the last few years.
Lots of new web standards have solved many problems that once required frameworks like jQuery to get
anything to work.

[React](https://react.dev/) and similar frameworks introduced new concepts, like components, and created workarounds to optimize browser rendering, like the Virtual DOM. 

Today, browsers have already implemented incredible optimizations and also added new standards for web components, templates,
routing, and much more.

For this reason, the UIX framework encourages the use of native Web APIs and does not focus that much on HTML rendering and browser optimizations. 

Our focus is on the next frontier: Completely erasing the gap between the backend and frontend.

Within the [unyt project](https://unyt.org), UIX plays a key role as the user-facing part the of the supranet.
The framework is developed hand in hand with the DATEX Protocol and optimized for distributed applications with end-to-end communication.

## How UIX works

UIX is essentially an extension of the [DATEX JS Runtime](https://docs.unyt.org/manual/datex/introduction). 
It adds new type interfaces for native HTML elements and custom component classes.

This means that reactivity is supported on a per-element basis like for any other DATEX value.

Because of this, you can already get far by just using the `$$()` method to bind DOM elements to a DATEX pointer,
and without explicitly using any UIX specific functions.

This is one of the main design goals of UIX: 

*We want to keep everything as close to the built-in browser APIs as possible, while still improving the developer and user experience significantly*.

Of course, UIX and the DATEX runtime environment still cause a considerable overhead with regard to loading times, but this can be mitigated with server prerendering and hydration.


## Comparison to React

In the following, we will take a look at a simple list component, implemented in React and UIX.

### React:
```tsx
declare const list: {name:string, url: string}[];

function ReactListView () {
	const [index, setIndex] = useState(0);
	const entry = list[index];

	return (<>
		<button onClick={()=>setIndex(index + 1)}>
			Next
		</button>
		<h2>
			<i>{entry.name} </i> 
			more: {entry.url}
		</h2>
		<h3>  
			({index + 1} of {list.length})
		</h3>
	</>);	  
}
```

### UIX:
```tsx
declare const list: {name:string, url: string}[];

function UIXListView() {
	const index = $$ (0);
	const entry = always(() => list[index]);

	return (<>
		<button onclick={()=>index.val++}>
			Next
		</button>
		<h2>
			<i>{entry.$.name} </i> 
			more: {entry.$.url}
		</h2>
		<h3>  
			({always(() => index + 1)} of {list.length})
		</h3>
	</>);	  
})
```
#### Explanation of the UIX component:
First, we create a new pointer for the index counter. 
This can somehow be compared to useState, but it creates a 
permanent reference to the value that can also be accessed and modified globally when required.
```tsx
const index = $$ (0);
```
Then, we create a new calulcated pointer with the `always` function.

```tsx
const entry = always(() => list[index]);
```
Any changes to the `index` pointer are immediately reflected in the `entry` pointer - it always contain the list value at the current `index` value.


In the `onclick` handler of the button, we increment the index value. This automatically updates all relevant pointers that depend on `index`.
```tsx
<button onclick={()=>index.val++}></button>
```

To get pointer references to the property values of the `entry` value, we use the special `$` property:
```tsx
<i>{entry.$.name} </i> 
```
If we just use `entry.name`, we will only get the current value of the property and the DOM wouldn't update when
the property is modified.

Finall, we create another pointer with `always`, holding the value `index + 1`:
```tsx
always(() => index + 1)
```

## JSX in UIX

We decided to add [JSX](https://www.typescriptlang.org/docs/handbook/jsx.html) support to UIX, because it has a readable syntax and good TypeScript support.

In contrast to frameworks like React, JSX in UIX is essentially just a wrapper around `document.createElement`
that also binds the element to a DATEX pointer.

```tsx
const count: Datex.Pointer<number> = $$(0);
const div: HTMLDivElement = 
	<div>
		<p>Count: {count}</p>
	</div>
```

If you don't want to use JSX, you can also use the `HTML` function which provides the exact same functionality as JSX with JavaScript template strings:
```tsx
const count: Datex.Pointer<number> = $$(0);
const div: HTMLDivElement = HTML`
	<div>
		<p>Count: ${count}</p>
	</div>`
```

## The key advantages of UIX

### UIX components are built on existing browser APIs

UIX does not need a virtual DOM. Component functions are always just called once for each component instance. No weird restrictions apply inside a component function. Everything behaves like normal JS.

### Everything is a pointer
 * Every component is a pointer
 * Every HTML element is a pointer
 * Every resource is a pointer
 * Every value is a pointer

Changes to pointer values are immediately reflected in connected observers (e.g. DOM elements), everything else remains unchanged.
If a pointer is not observed anywhere, there is no significant overhead.

### A component or parts of its state can be shared between endpoints

=> Components can exist in a shared context between backend and frontend

=> Anonymous function components already support frontend and static/dynamic backend rendering per default. Hydration is supported for class components.
