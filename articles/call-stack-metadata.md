<!--
	{
		description: "Or: how to abuse JavaScript error stack traces",
		preview: "res/uix_banner.png",
		date: ~2023-07-18~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->

# How to Pass Hidden Metadata to Function Calls - A Journey

Now and then, we as developers seamingly reach the frontier of possible functionality of a programming language.

*Really, I cannot implement an HTML syntax tree parser with typescript template strings?*
...


Compared to many other languages out there, no one would say that JavaScript is very restricted in regards to flexiblity.
Nearly everything can be modified and adapted to very specific usecases.
I mean, we can completely override the f*ckn prototype of an object after creation, a point at which even TypeScript has to silently resign.
JavaScript has powerful reflection capabilities.
But sometimes, even JavaScript has no answer to a problem.

## The problem

**Imagine the following simple scenario**:
You have a function that takes a path to a file as an argument and tries to read its content.
This could be in a browser, where the file is fetched from a URL, or on a backend like node or deno, where the file is directly
read from the file system.
```ts
function readFile(path: string) {
	// reading the content somehow...
}

readFile("/tmp/image.png")
```

As it turns out, Deno already provides such a function in the standard library:
```ts
const content = await Deno.readFile("/tmp/image.txt")
```

Similarly, the browser `fetch` API allows us the get the content of a URL path - So far, so good.
Let's try something different:
```ts
const content = await Deno.readFile("./assets/image.txt")
```

## We are not Deno

Doesn't look too spectacular, right?
We are now requesting a relative file path, relative to the location of the current module.
Deno will still resolve this path correctly, but if we think about if for a bit, we cannot replicate this functionality with our custom `readFile` function.

The problem is that we only pass a relative path, but in contrast to Deno, which can play some tricks with its own runtime, we are not able to determine in our `readFile` function *from which module* it was called and thus we don't know from which
*absolute module* path the relative path should be interpreted.

## A possible solution

Actually, there is a (relativly new) ES feature that can help us to solve this problem:
```ts
import.meta.url
```
This special property returns the URL of the current module.
For our usecase, we would have to pass this value as an additional argument to the `readFile` function:
```ts
readFile("./assets/image.txt", import.meta.url)
```

This is definitly an OK solution, but is not as clean as the `Deno.readFile` function.
There is some unnessary overhead, and developers have to keep in mind that they always need to pass in
`import.meta.url`, and nothing else, as the second argument.

Can't we do this better somehow?

## Entering the realm of *non-standard* features

Maybe you've heard of the bespoke [`Function.prototype.caller`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/caller) property, that allowed an invoked function ('callee') to get a reference to the function from which it was called ('caller').
This feature has been marked as deprecated for a long time, and is also a *non-standard* feature.

Another, currently *non-standard*, feature is the [`Error.prototype.stack`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/stack). Other than the function caller property, this is not a deprecated feature, and there are ongoing proposals to create a common standard.
It is somewhat incredible that a feature that has existed for most of the existence of the web has not yet been standardized, but..


## Fun with browser incompatibilities

## Come on, Safari

I don't want to hate on Safari too much. They have come a long way in the last few years, but
it is still incredible how often they lag behind with implementing basic features.
One feature, which the Safari dev team was probably very proud of, are *tail call optimizations* "Now only in Safari!". Great. This time, Safari was faster than the competition. Before there even was a standard for this behaviour. Perfect.
Incompatibilities incoming.

What's the problem with tail call optimizations?
Well, there's not really a problem. Tail call optimizations (TCOs) allows for a much more efficient
execution of specific recursive functions...
:


This does not only create a big problem for our call stack hack, it is generally not
great for debugging, when functions with tail calls are involved.

The only workaround for this is to explicitly store the result of the called function (in our case `readFile` in a variable, and then return that variable afterwards)


## Taking it further: Passing hidden metadata with call stacks
