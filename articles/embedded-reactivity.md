<!--
	{
		description: "",
		preview: "res/uix-banner-jsx.png",
		date: ~2023-12-04~,
		tag: "Developer",
		author: "Benedikt Strehle",
		authorRef: https://github.com/benStre
	};
-->


# Introducing: Experimental embedded JSX reactivity

*The features described in this article are still in an early stage of development. The behaviour might change significantly in the future and the functionality could be completely removed.*

## Overview of the current DATEX/UIX reactivity model

DATEX JS has a fine-grained reactivity model that provides inherent reactivity for values wrapped with `$$()`.

Updating reactive values works exactly the same as with normal values:
```ts
const x = $$({a: 41, b: 42});
x.a = 43;
```

Reactive computations can be defined with special transform functions:
```ts
import { add } from "unyt_core/functions.ts";

const a = $$(1);
const b = $$(2)
const sum = add(a, b); // Datex.Ref<3>
```

Alternatively, you can use the generic `always()` transform function containing vanilla JavaScript operations:
```ts
const a = $$(1);
const b = $$(2)
const sum = always(() => a + b); // Datex.Ref<3>
```

Primitive reactive values can directly be passed to JSX elements:
```tsx
const x = $$("Hello");
export default <div>{x}</div>
```

To bind live object properties to a JSX element, the `$` property syntax is required:
```tsx
const x = $$({content: "Hello"});
export default <div>{x.$.content}</div>
```

You can achieve the same behaviour using `always()`:
```tsx
const x = $$({content: "Hello"});
export default <div>{always(() => x.content)}</div>
```

## Embedded JSX reactivity

With the new experimental embedded JSX reactivity feature, it is no longer required to explicitly
use `always()` inside JSX definitions. The example above can be simplified to:

```tsx
const x = $$({content: "Hello"});
export default <div>{x.content}</div>
```

This also works with more complex expressions:

```tsx
const show = $$(false);
export default <div>{show ? "Hello World" : ""}</div>
```

```tsx
const counter = $$(0);
export default <div>Next Value: {counter + 1}</div>
```

## The $() syntax

For reactivity outside of JSX definitions, we are introducing the new `$()` shorthand function
which could replace both the `$$()` and the `always()` function in the future.

Example using `$$()` and `always()`:
```ts
const x = $$(10);
const y = always(() => x * 2);
```

This can now be written using `$()`:
```ts
const x = $(10);
const y = $(x * 2);
```



## Caveats

* Both the new JSX reactivity and the `$()` syntax are extending
the TypeScript/TSX standard and do not work in a default TypeScript/TSX
implementation.<br/>
They are implemented with [SWC](https://swc.rs/) plugins and work like macros that are transformed at compile time.

* Embedded JSX reactivity is currently internally provided with `always()` transforms that are not as efficient as dedicated transform functions or `$` methods:
	```tsx
	const list = $$([...]);

	// new embedded JSX reactivity
	export default <div>{list.map(item => item.name)}</div>

	// $ method (more efficient)
	export default <div>{list.$.map(item => item.name)}</div>
	```
 	This will be improved in the future.

## How to use it

Currently, the `$()` syntax as well as the embedded JSX reactivity functionality
are not enabled per default.

If you want to try out these features, you can enable them as experimental features in
your `app.dx` file:

```datex
name: "My App",
experimentalFeatures: ['embedded-reactivity'];
```

Keep in mind that the embedded reactivity features are currently only supported for *frontend modules*.

Please report any issues on https://github.com/unyt-org/uix or https://github.com/unyt-org/jusix.
