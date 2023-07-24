<!--
	{
		description: "Seamless Backend and Frontend Routing with UIX",
		preview: "res/uix_banner.png",
		date: ~2023-07-18~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->

# Exploring the Power of Routing in the UIX Framework

Routing has always been a core issue in web server frameworks, and also in an increasing amount of frontend web frameworks supporting Single-Page Applications.

Over the years many approaches for where established.
React is embracing Routing with JSX Components.

...

## Entrypoints

In UIX, every route is resolved starting from the `default` export in the `entrypoint.ts` file.
This default export value can contain various different values, as explained in the following chapter. Valid values satisfy the `UIX.Entrypoint` type.

A `UIX.Entrypoint` can be any renderable content, like:
 * A Component or simple HTML Element
 * A primitive value, like a string or number
 * A `Response` object

Displaying plain text:
```tsx
export default "Hello World";
```

Instead of static content, you can also always provide an (async) function returning the content instead:

```tsx
export default () => `Hello World, the time is ${new Date().toLocaleString()}`;
```

Keep in mind that this dynamic content is only generated once when the entrypoint is loaded.
If you want live content, you can pass in reactive values (Datex Pointers), for example by creating a `text` pointer:

```tsx
const currentTime: Datex.Pointer<Date> = Datex.Time.getLive();
export default () => text `Hello World, the time is ${currentTime}`;
```

Besides that, entrypoints can be an implementation of `UIX.RouteHandler`.

## Route Handlers
A route handler is an entrypoint that takes a route as an input and returns a new entrypoint for that route.


UIX strictly regards JSX as another way to represent DOM Elements, and thus does support routing via JSX Components.


## File-Based Routing

