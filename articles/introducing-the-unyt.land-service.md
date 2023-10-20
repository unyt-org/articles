<!--
	{
		description: "We introducte the brand new unyt.land service - your gateway to seamless TypeScript module loading directly in the browser.",
		preview: "res/unyt-land.png",
		date: ~2023-10-20~,
		tag: "Developer",
		author: "unyt.land",
		authorRef: https://unyt.land
	};
-->


# Introducing: The unyt.land Webproxy
In the world of web development, one of the most significant hurdles we face is the need to transpile **TypeScript** to **JavaScript** before running it in the browser. This extra compilation step can be cumbersome, time-consuming... and sucks, yeah!

What if there was a way to eliminate this step altogether and work directly with **TypeScript** in your web applications? Let us introducte `unyt.land`, the revolutionary web proxy that simplifies your TypeScript development workflow by enabling you to import TypeScript modules directly in the browser.

## Enabling Cross-Environment Development
Web developers frequently work in multiple environments, such as Deno, Node.js, and the browser. Our service transcends these boundaries by making TypeScript modules accessible across various environments, thus promoting **code reuse** and reducing the need to create multiple versions of the same module for different platforms.

## Features
Our service comes equipped with those exciting features:
* Automatic on-the-fly compilation
* TypeScript support
* SASS support
* JSX support
* SourceMaps

This means you can effortlessly load modules from common CDNs such as [deno.land](https://deno.land) in the browser - just rename `deno.land` to `unyt.land`!

## How to use unyt.land?
1. Refactor your `deno.land` imports to use `unyt.land` instead:
	```ts
	import mod from "https://deno.land/x/mod.ts"
	```
	to
	```ts
	import mod from "https://unyt.land/x/mod.ts"
	```
2. If you plan to run a module on both *deno* and *browser* environments, please make sure that the module **does not** use [deno-only APIs](https://deno.land/api@v1.37.2) (such as FileSystem or Network). Otherwise please polyfill those functionality for the browser environment to avoid runtime exceptions.
3. *You are ready!* Enjoy improved performance, reusability and efficiency with automatic TypeScript compilation provides by [unyt.land](https://unyt.land).

## Conclusion
In an age where efficient development is paramount, "unyt.land" emerges as a powerful ally for web developers. It bridges the gap between TypeScript and the browser, facilitating smoother, faster, and more efficient development. Give "unyt.land" a try and experience the future of web development today ❤️!

## References
* [unyt.org](https://unyt.org)
* [unyt.land](https://unyt.land)