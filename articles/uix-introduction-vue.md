<!--
	{
		description: "An introduction to the UIX framework for Vue.js developers",
		preview: "res/uix_banner.png",
		date: ~2025-11-01~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->


# Getting started with UIX – coming from Vue
Welcome to UIX! If you have used [Vue](https://vuejs.org) before, this guide is for you.

UIX's reactivity behaves very similarly to Vue's. In fact, it is implemented on top of the same [JavaScript mechanism](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) that drives Vue 3. The syntax, however, is different, albeit based on familiar principles.

While JavaScript is popular with Vue developers, UIX builds solely on [TypeScript](https://typescriptlang.org). It is a type-safe superset of the JavaScript programming language, meaning that any code of the former is also valid code for the latter.

## Running the live server
<unyt-tabs>
<unyt-tab label="Vue">

With Vue 3, you can start a hot-reloading live server with
```sh
npm run dev
```

</unyt-tab>
<unyt-tab label="UIX" default>

With UIX, you use
```sh
uix -l
```
to start a live server instance with hot-reloading enabled.

</unyt-tabs>

## App.vue
When you visit your server's main page, this is the first thing that is rendered. UIX has a similar concept. Here, we call it _entrypoint_. As UIX is a full-stack framework, `backend/entrypoint.ts` will be executed when the server starts and `frontend/entrypoint.tsx` will be rendered for every web browser that requests your website.

<unyt-tabs>
<unyt-tab label="Vue">

A Hello World Vue app would look like:
```vue
<!-- src/App.vue -->
<template>
<h1>Hello World</h1>
</template>
```

</unyt-tab>
<unyt-tab label="UIX" default>

A Hello World UIX app would look like:
```tsx
export default <h1>Hello World</h1>
```
</unyt-tab>
</unyt-tabs>

## JSX: Dynamic Data in HTML
[JSX](https://legacy.reactjs.org/docs/introducing-jsx.html) is a syntax extension for JavaScript. It is also used in [React](https://react.dev) and allows developers to define HTML code right inside JavaScript / TypeScript. The special thing is that said HTML code will not be a bare string but an actual HTMLElement instance that can be added to the DOM. It is therefore possible to assign HTML elements to variables like so:
```tsx
const xyz = <div>
    <span>I will be used somewhere else later</span>
</div>;
```

To create an HTML element with JSX, one root element will always be required (e.g. `div`, as seen above). If, at any point in time, you wish to render literal text without markup in the browser, you can use bare JavaScript strings:
```tsx
const xyz = "I will be used somewhere else later";
```

### Inserting TypeScript Data into HTML
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you can use the `{{ template }}` syntax to insert JavaScript expressions into HTML:
```vue
<!-- src/App.vue -->
<script setup>
	const subject = "World";
</script>
<template>
	<main>Hello, {{ subject }}!</main>
</template>
```

</unyt-tab>
<unyt-tab label="UIX" default>

With the JSX syntax that is used in UIX, you only use one curly brace on each side:
```tsx
// frontend/entrypoint.tsx

const subject = "World";
export default <main>Hello, { subject }!</main>;
```

</unyt-tab>
</unyt-tabs>

### Conditionals
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you can use the `v-if` directive to conditionally render HTML:
```vue
<!-- src/App.vue -->
<script setup>
	const sayHello = true;
</script>
<template>
	<main>
		<p v-if="sayHello">Hello, World!</p>
       <p v-else>Something Else!</p>
	</main>
</template>
```

<unyt-tab label="UIX" default>

In UIX, which uses the JSX syntax, only JavaScript expressions are allowed. As `if` and `else` are statements and not expressions, the JavaScript [Conditional Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) shall be used instead:
```tsx
// frontend/entrypoint.tsx
const sayHello = true;
export default <main>
	{ sayHello ? <p>Hello, World!</p> : <p>Something Else!</p> }
</main>;
```

An alternative that is less redundant would be:
```tsx
// frontend/entrypoint.tsx
const sayHello = true;
export default <main>
	<p>{ sayHello ? "Hello, World!" : "Welcome to UIX!" }</p>
</main>
```

### Loops
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you can use the `v-for` directive to render elements based on an array:
```vue
<!-- src/App.vue -->
<script setup>
    const data = [
        { todo: "Learn UIX", done: true },
        { todo: "Become unyt.org member", done: true }
    ];
</script>
<template>
	<main>
		<h1>ToDo List</h1>
		<div v-for="item in data">
            {{ item.todo }} - {{ item.done ? "Done" : "Outstanding" }}
        </div>
    </main>
</template>
 ```
</unyt-tab>
<unyt-tab label="UIX" default>

In UIX, you use the standard JavaScript function [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) to convert an array of items into an array of HTML elements. UIX will automatically render an array of HTML elements as multiple consecutive elements in the browser:

```tsx
// frontend/entrypoint.tsx
const data = [
    { todo: "Learn UIX", done: true },
    { todo: "Become unyt.org member", done: true }
];

export default <main>
	<h1>ToDo List</h1>
	{ data.map(item => <div>{ item.todo } - { item.done ? "Done" : "Outstanding" }</div>) }
</main>
```

In the example above, each item of the data array gets mapped to a corresponding div.
</unyt-tab>
</unyt-tabs>

## Reactivity
Vue and UIX share some similarities when it comes to two-way data binding. Both are based on the underyling [JavaScript Proxy System](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### Making a Variable Reactive
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you use `ref()` to make primitive values reactive. For complex data types on the other hand, you use `reactive()`:

```vue
<!-- src/App.vue -->
<script setup>
    import { ref, reactive } from "vue";
	 const counter = ref(0);
    const data = reactive([
        { todo: ..., done: ...},
        ...
    ]);
</script>
```
</unyt-tab>
<unyt-tab label="UIX" default>

In UIX, you use the all-in-one reactivity function `$()` to make any kind of expression reactive:
```tsx
// frontend/entrypoint.tsx
const counter = $(0);
const data = $([
    { todo: ..., done: ...},
    ...
]);
 ```
</unyt-tab>
</unyt-tabs>

### Two-way data binding
Certain elements like `input` support bidirectional data binding.
<unyt-tabs>
<unyt-tab label="Vue">

With Vue, you can use the `v-model` directive to bind a reactive variable bidirectionally:
```vue
<!-- src/App.vue -->
<script setup>
    import { ref } from "vue";
    const name = ref("");
</script>
<template>
    <main>
        Your name: <input type="text" v-model="name"></input> <br>
        (Your name in uppercase: {{ name.toUpperCase() }})
    </main>
</template>
```
</unyt-tab>
<unyt-tab label="UIX" default>

With UIX, you use HTML standard attribute names instead of `v-model` attributes to bind data bidirectionally:
```tsx
// frontend/entrypoint.tsx

const name = $("");
export default <main>
    Your name: <input type="text" value={ name } /> <br />
    (Your name in uppercase: { name.toUpperCase() })
</main>;
```
</unyt-tab>
</unyt-tabs>

### Computed Variables
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you use the `computed()` function to make a variable have a certain value that is updated whenever its dependent data changes:
```vue
<!-- src/App.vue -->
<script setup>
    import { ref, computed } from "vue";
    const a = ref(39);
    const b = ref(3);
    const c = computed(() => a.value + b.value);
</script>
```
</unyt-tab>
<unyt-tab label="UIX" default>

In UIX, you use the all-in-one reactivity function `$()` to declare any kind of reactively recomputable expression:
```tsx
// frontend/entrypoint.tsx
const a = $(39);
const b = $(3);
const c = $(a + b);
```
The UIX compiler automatically configures `c` to be updated whenever `a` or `b` changes.

If you did not want to use UIX-specific compiler functionality, you could have written:
```tsx
const c = always(() => a + b);
```
which would have done a similar thing and is an alternative way to express the same.
</unyt-tab>
</unyt-tabs>

## Components
<unyt-tabs>
<unyt-tab label="Vue">

With Vue, you would commonly define components with props using the Vue Single File Component (SFC) Format:
```vue
<!-- src/components/ListItem.vue -->
<script setup>
	const props = defineProps({
	    todo: String,
       done: boolean
	});
</script>

<template>
	<div>{{ props.todo }} - {{ props.done ? "Done" : "Outstanding" }}</div>
</template>

<style>
	div { font-size: 0.9em; }
</style>
```
</unyt-tab>
<unyt-tab label="UIX" default>

With UIX, you use the already introduced TSX (TypeScript) file tyoe to define execution logic, HTML content and styles in a single file:
```tsx
// frontend/components/ListItem.tsx
import { Component } from "uix/components/Component.ts";

@template(function(props) {
	return <div>{ props.todo } - { props.done ? "Done" : "Outstanding" }</div>;
})
@style(css `
    div { font-size: 0.9em; }
`)
export class ListItem extends Component<{
	 todo: string,
    done: boolean
}> {
    /* Place per-component-instance logic, lifecycle hooks,
       temporary variables, methods etc. here. */
};
```

In UIX, components are plain TypeScript objects that inherit from the `Component` base class. They can optionally be decorated with a `template` and a `style`. Props are declared in the class declaration and can be used inside the class, the template and the style.
</unyt-tab>
</unyt-tabs>

### Using Components and Binding Values to Attributes/Props
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you import components and use the `v-bind` (abbreviated as `:`) directive to render them with props:
```vue
<!-- src/App.vue -->
<script setup>
    import ListItem from "./components/ListItem.vue";
    import { reactive } from "vue";
	const example = reactive({ todo: "Learn UIX", done: true });
</script>
<template>
    <ListItem v-bind:todo="example.todo" :done="example.done" />
</template>
```
</unyt-tab>
<unyt-tab label="UIX" default>

In UIX, you import components and use curly braces to render them with props:
```tsx
// frontend/entrypoint.tsx
import { ListItem } from "./components/ListItem.tsx";

const example = $({ todo: "Learn UIX", done: true });
export default <ListItem todo={ example.todo } done={ example.done } />;
```
</unyt-tab>
</unyt-tabs>

### Emitting Events
<unyt-tabs>
<unyt-tab label="Vue">

In Vue, you use dedicated event emissions and event listeners to communicate action from a component to its parent:

```vue
<!-- src/components/ListItem.vue -->
<script setup>
    // ...
    const emit = defineEmits(["delete"]);
</script setup>
<template>
    <div>
        ...
        <button @click="emit('delete', props.todo, props.done)">Delete</button>
    </div>
</template>
```

```vue
<!-- src/App.vue -->
<script setup>
	import ListItem from "./components/ListItem.vue";
    // ...
    function handleDeletion(todo, done) {
        // TODO: Process Deletion
    }
</script>
<template>
    <ListItem :todo="example.todo" :done="example.done" @delete="handleDeletion" />
</template>
```

</unyt-tab>
<unyt-tab label="UIX" default>

In UIX, there is no dedicated event system. In fact, you can easily resemble Vue's event functionality using a pure standardized JavaScript feature – Callbacks:
```tsx
// frontend/components/ListItem.tsx
import { Component } from "uix/components/Component.ts";

@template(function(props) {
    return <div>
        ...
        <button onclick={ props.ondelete(props.todo, props.done) }>Delete</button>
    </div>;
})
@style(css `
    div { font-size: 0.9em; }
`)
export class ListItem extends Component<{
	 // ...
    ondelete: (todo: string, done: boolean) => void
}> {
    /* Place per-component-instance logic, lifecycle hooks,
       temporary variables, methods etc. here. */
};
```

Simply specify a component prop like `ondelete` as a callback function. TypeScript allows you to strictly type the signature of this function. Then, use it in the parent environment:

```tsx
// frontend/entrypoint.tsx
import { ListItem } from "./components/ListItem.tsx";

// ...

function handleDeletion(todo: string, done: boolean) {
    // TODO: Process Deletion
}

export default <ListItem todo={ example.todo } done={ example.done } ondelete={ handleDeletion }/>;
```
</unyt-tab>
</unyt-tabs>

## Additional Features
### Routing
For Vue, you use one of the external routing libraries. UIX supports Single-Page Application Routing out of the box and features a versatile routing system. In its simplest form, it performs map-based routing:
```tsx
// frontend/entrypoint.tsx

export default {
    "/login": () => import("./login.tsx"),
    "/todos": () => import("./todos.tsx"),
    "/:userId/info": (_, params) =>
        <main>
            <h1>User Information</h1>
            For User ID { params.userId }
        </main>
};
```
```tsx
// frontend/login.tsx
export default <main>
    ...
</main>
```

Beyond that, UIX also supports file-based routing (although discouraged for larger projects) and dynamic route processing. Links can be rendered using the `<a href>` tag.
### Backend Rendering
To use the [integrated UIX backend rendering](https://docs.unyt.org/manual/uix/rendering-methods), with hydration enabled by default, do not use `frontend/entrypoint.tsx` but `backend/entrypoint.tsx` to render your HTML statically on the server. The syntax remains the same. To keep your projects intuitively organized, move your components from `frontend/...` to `backend/...` or `common/...`.

### Realtime Data Synchronization
To abstract away the necessity of manually transferring realtime data across devices using JSON, BSON, Remote Procedure Calls, WebSockets and similar technologies, UIX was designed to streamline this process deeply at its core. It employs a new realtime-data-centric communication protocol called [DATEX](https://docs.unyt.org/manual/datex) – a technology developed and implemented by the same [organization that created UIX](https://unyt.org).

Based on this underlying technology, UIX provides a feature called [Cross Realm Imports](https://docs.unyt.org/manual/uix/cross-realm-imports).

1. Create a reactive value (DATEX calls it a "pointer") on the backend.
   ```tsx
   // backend/data.ts
   const todo = $("Learn UIX");
   ```
2. Now, the variable is reactive on the backend. Export it.
   ```tsx
   // backend/data.ts
   export const todo = $("Learn UIX");
   ```
3. Import and use it on the frontend.
   ```tsx
   // frontend/entrypoint.tsx
   import { todo } from "backend/data.ts";
   export default <main>
       Global ToDo: <input value={ todo } />
   </main>
   ```

Now, the variable is **reactively synchronized across all website visitors in realtime** with bidirectional data binding. You can also export functions and call them anywhere else. Note that you must always `await` those variables and functions if you wanted to use and process them in JavaScript/TypeScript – behind the curtain, they are network transmissions after all.

### Databases

UIX includes permanent data storage mechanisms that obviate the need for a database.

1. Create a reactive value (DATEX calls it a "pointer")
   ```tsx
   // backend/data.ts
   export const users = $({
       example: [...]
   });
  ```
2. Annotate it with the `eternal ??` label
   ```tsx
   // backend/data.ts
   export const users = eternal ?? $({
       example: [...]
   });
  ```

Now, data is permanently stored across backend restarts. Execute `uix --clear` the stored records in your database.

Marking a pointer on the frontend with `eternal ??` will persist it in the device's `localStorage`. As opposed to JSON, the underlying DATEX technology allows you to store any arbitary value, including primitives, maps, class instances, callbacks, HTML elements and more.