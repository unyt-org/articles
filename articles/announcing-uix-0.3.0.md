<!--
	{
		description: "Introducing magic with UIX@0.3.0",
		preview: "res/uix-0.3.0.png",
		date: ~2024-10-15~,
		tag: "Developer",
		author: "Jonas Strehle",
		authorRef: https://github.com/jonasstrehle
	};
-->

# Introducing magic with UIX@0.3.0

The latest release of UIX is here, and it’s not just an upgrade — it’s an evolution. With [UIX@0.3.0](https://github.com/unyt-org/uix), reactivity is no longer something you have to think about. *It’s simply there, by default. Like magic!*

To make this experience, we’ve introduced [JUSIX](https://github.com/unyt-org/jusix), a Rust-based module that handles the interpretation of JSX code as reactive JavaScript. UIX uses a custom version of Deno as backend runtime called [Deno for UIX](https://github.com/unyt-org/deno). JUSIX is integrated directly into the [Deno for UIX AST](https://github.com/unyt-org/deno_ast) (Abstract Syntax Tree) to enable automatic reactivity for the backend. JUSIX does also work for frontend *(browser)* code by transpiling the frontend modules to plain JavaScript using SWC with the [JUSIX WASM plugin](https://github.com/unyt-org/jusix/tree/wasm-plugin) enabled. That allows the browser to treat reactivity the same way as the backend does. 

That’s right — you don’t even need to wrap your head around complex logic using `$`-properties and `always` calls as required for older UIX releases. UIX does now take care of it for you!


Here’s a simple example:

```tsx
const counter = $(0);
<button
	value={'Clicked:' + counter}
	onclick={() => counter.val++}/>;
```

This `<button>` is bound to the counter `Ref`. When the button is clicked, the counter value updates automatically, and so does the button’s label. We don’t need to write any additional logic.

## The “always” Method? Throw it away! 🪄

In earlier versions, UIX required you to manually use the always method to handle reactive updates:

```tsx
const myVar = $(4);
<div>{always(() => myVar + 1)}</div>;
myVar.val ++;
```

But in `UIX@0.3.0`, this is all handled for you. The new syntax can look like this:

```tsx
const myVar = $(4);
<div>{myVar + 1}</div>;
myVar.val ++;
```

It’s pure reactivity, with zero hassle. We took care of it so you don’t have to. Find out more in our [docs](https://docs.unyt.org/manual/uix/getting-started).

## UIX@0.3.0 Changelog 📝
We didn’t just stop at improving reactivity. Here’s what else you can expect in this release:

* **Performance Improvements**: Various optimizations were made to boost performance, especially for large-scale applications with complex data structures.
* **Bug Fixes**: We’ve squashed some bugs that had been lurking around.
* **Import Restructuring**: We cleaned up our imports, making it easier for you to follow best practices when building your apps.
* **New `provideImage` Method**: Need to render your HTML components as images? This is now possible, similar to Next.js’s Image component. Generate server-side images right from your UIX app!
* **New Install Script**: No more manual installations of Deno required. Run the new [custom install script](https://github.com/unyt-org/uix-install), and it’ll take care of installing both Deno for UIX and UIX in one go.


## A Love Letter to Deno 💖
>Dear Deno,
>
>You had me at **no `node_modules`**! Your native TypeScript support, security-first design, and sleek, clean repository structure — what’s not to love? Thanks to your design it was effortless for us at UIX to create our custom [Deno fork](https://github.com/unyt-org/deno) that even uses your CI.
>
>But this letter it’s not just about us. You’ve been the foundation for UIX - our fullstack framework that we are really proud of. Deno, over the years you’ve been more than a runtime to us — you’re the reason UIX exists.
>
>With love,<br/>
>The UIX Team

*Alright, alright - putting aside the seemingly AI generated letter above - a big thank you to the Deno team - you are doing a great job and we have been fans of your project since day one.*

## Ready to Dive In? 🚀
The future of web development with UIX is now even more reactive, more efficient, and easier to use than ever before. Whether you’re working on your apps backend logic or working on your highly dynamic frontend, `UIX@0.3.0` has got you covered.

So, what are you waiting for? [Install UIX!](https://github.com/unyt-org/uix-install)

Yours reactively,<br/>
The unyt.org Team 