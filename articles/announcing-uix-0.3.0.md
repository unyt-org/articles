<!--
	{
		description: "Introducing magic with UIX 0.3",
		preview: "res/uix-0.3.png",
		date: ~2024-10-15~,
		tag: "Developer",
		author: "Jonas Strehle",
		authorRef: https://github.com/jonasstrehle
	};
-->

# Introducing magic with UIX 0.3

The latest release of UIX is here, and it’s not just an upgrade — it’s an evolution. With [UIX 0.3](https://github.com/unyt-org/uix), reactivity is no longer something you have to think about. *It’s simply there, by default. Like magic!*

To make this experience, we’ve created [JUSIX](https://github.com/unyt-org/jusix), an [SWC](https://swc.rs/) plugin that handles the interpretation of JSX code as reactive JavaScript. UIX uses a custom version of Deno as backend runtime called [Deno for UIX](https://github.com/unyt-org/deno). JUSIX is integrated directly into the [deno_ast crate](https://github.com/unyt-org/deno_ast) to enable automatic reactivity for the backend. JUSIX also works for frontend *(browser)* code by transpiling the modules to plain JavaScript using a [SWC WASM plugin](https://github.com/unyt-org/jusix/tree/wasm-plugin). That allows the browser to treat reactivity the same way as the backend does. 

Now you no longer need to wrap your head around complex logic using `$`-properties and `always` calls as required for previous UIX versions. UIX does now take care of it for you!

Here’s a simple example:

```tsx
const checked = $(false);
export default
    <div>
        <input type="checkbox" checked={checked}/>
        <label>{checked ? "Checked" : "Not checked"}</label>
    </div>
```

This checkbox is bound to the `checked` pointer, meaning that the value of `checked` is updated when the checkbox is checked/unchecked. The label text always reflects the current state of the checkbox - the ternary
expression is automatically reevaluted when the value of the `checked` pointer changes.
We don’t need to write any additional logic.

## The “always” Method? Throw it away! 🪄

In earlier versions, UIX required you to manually use the always method to handle reactive updates:

```tsx
const myVar = $(4);
<div>{always(() => myVar + 1)}</div>;
myVar.val ++;
```

But in UIX 0.3, this is all handled for you. The new syntax can look like this:

```tsx
const myVar = $(4);
<div>{myVar + 1}</div>;
myVar.val ++;
```

It’s pure reactivity, with zero hassle. We took care of it so you don’t have to. Find out more in our [docs](https://docs.unyt.org/manual/uix/getting-started).

## UIX 0.3 Changelog 📝
We didn’t just stop at improving reactivity. Here’s what else you can expect in this release:

* **Performance Improvements**: Various optimizations were made to boost performance, especially for large-scale applications with complex data structures.
* **Bug Fixes**: We’ve squashed some bugs that had been lurking around.
* **Import Restructuring**: We cleaned up our imports, making it easier for you to follow best practices when building your apps.
* **New `provideImage` Method**: Need to render your HTML components as images? This is now possible, similar to Next.js’s Image component. Generate server-side images right from your UIX app!
* **New Install Script**: No more manual installations of Deno required. Run the new [custom install script](https://github.com/unyt-org/uix-install), and it’ll take care of installing both Deno for UIX and UIX in one go.


## A Love Letter to Deno 💖
> Dear Deno,
>
> You had me at **no `node_modules`**! Your native TypeScript support, security-first design and focus on web standards - what’s not to love?
> Thanks to your fantastic work, it was effortless for us at unyt.org to create our custom [Deno fork](https://github.com/unyt-org/deno) that even uses your CI.
>
> But this letter is not just about us. You’ve been the backbone of the UIX project. Deno, over the years you’ve been more than a runtime to us — you’re the reason UIX exists.
> Congratulations to your release 2.0!
>
>With love,<br/>
>The UIX Team

*Alright, alright - putting aside the seemingly AI generated letter above - a big thank you to the Deno team - you are doing a great job and we have been fans of your project since day one.*

## Ready to Dive In? 🚀
The future of web development with UIX is now even more reactive, more efficient, and easier to use than ever before. Whether you’re working on your apps backend logic or working on your highly dynamic frontend, `UIX@ 0.3` has got you covered.

So, what are you waiting for? [Install UIX!](https://github.com/unyt-org/uix-install)

Yours reactively,<br/>
The unyt.org Team 
