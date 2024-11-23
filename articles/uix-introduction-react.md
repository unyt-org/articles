<!--
	{
		description: "An introduction to the UIX framework for React developers",
		preview: "res/uix_banner.png",
		date: ~2023-11-3~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->

# Gettings started with UIX - coming from React

Welcome to UIX! If you have used [React](https://react.dev/) before, this article is for you.

UIX has many concepts similar to React, but it is based on a different approach.
With this article, we will try to make it easy for you to transition from React to UIX.
This article roughly follows the [React quick start guide](https://react.dev/learn).

## The fundamental difference between React and UIX

While React and UIX components might look very similar on first glance, the
underlying approach is fundamentally different:

While a React component function is executed again on each state change,
UIX component functions are only every executed once. To get dynamic updates for state changes,
UIX relies on fine-grained reactivity instead: State changes are passed through the data itself.

With this model, UIX still uses functional concepts, but gets rid of the often weird and "non-JavaScript-like"
behaviour of `useState`, `useEffect`, etc.
This also means that UIX does not need a virtual DOM, but works directly with the *actual* DOM instead.

Additionally, everything in UIX is fundamentally based on standard web APIs.
Within your UIX app, you can still use non-reactive APIs like `document.createElement`, `element.setAttribute` instead of JSX declarations.

## Let's start easy: JSX

Like React, UIX supports JSX for creating elements and components.
For the most basic use case - defining static UI - there aren't many differences between React and
UIX. 

Because UIX tries to support JSX that is as close to HTML as possible, there are 
a view small differences:
 * UIX uses the `class` attribute instead of `className`,
 * event handler attributes like `onclick` are also written in lowercase like they are normally written
 in HTML, not in camelCase (`onClick`) like in React.
 * UIX supports both objects and css strings for the `style` attribute


## Creating components

UIX components can be defined with functions, just like in React (In contrast to React, 
component classes are also still used in UIX).

The basic example from the React quick start guide can be used in UIX without any modifications:

```tsx
function MyButton() {
    return (
        <button>
            I'm a button
        </button>
    );
}

export default function App() {
    return (
        <div>
            <h1>Welcome to my app</h1>
            <MyButton />
        </div>
    );
}
```

The default export in the `entrypoint.tsx` file is rendered on the page.
In contrast to React, this doesn't have to be a function, but can also take
[lots of other values](./05%20Entrypoints%20and%20Routing.md#entrypoint-values).
For example, you can just directly return a div element:

```tsx
export default (
    <div>
        <h1>Welcome to my app</h1>
        <MyButton />
    </div>
);
```

## Displaying data

Following the React examples, the following code is also valid UIX:

```tsx
const user = {
    name: "Hedy Lamarr",
    imageUrl: "https://i.imgur.com/yXOvdOSs.jpg",
    imageSize: 90,
};

export default function Profile() {
    return (
        <>
            <h1>{user.name}</h1>
            <img
                class="avatar"
                src={user.imageUrl}
                alt={"Photo of " + user.name}
                style={{
                    width: user.imageSize,
                    height: user.imageSize,
                }}
            />
        </>
    );
}
```

As you can see, UIX supports fragments and all default element attributes.

## Responding to events

Event handlers can also be assigned to attributes like in React (keep in mind that all attributes are written in lowercase, meaning `onClick` becomes `onclick`):

```tsx
// UIX

function MyButton() {
    function handleClick() {
        alert("You clicked me!");
    }

    return (
        <button onclick={handleClick}>
            Click me
        </button>
    );
}

export default <MyButton />;
```

## Using state

Let's take a look at a simple React example using `useState`:
```tsx
// React

function MyButton() {
    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1);
    }

    return (
        <button onClick={handleClick}>
            Clicked {count} times
        </button>
    );
}
```

The `useState` hook returns two values, a `count` number
and and `setCount` function that has to be used to update the `count`
value.

UIX does not have the concept of hooks.
Instead, UIX has *pointers*, which are atomic reactive values
that can be used everywhere in a UIX app, not just within components.
A pointer is created with `$()`, passing in the initial value as an argument.

Primitive pointer values are updated by setting the `.val` property of the pointer.

```tsx
// UIX

function MyButton() {
    const count = $(0); // create new pointer

    function handleClick() {
        count.val++; // update pointer value
    }

    return (
        <button onclick={handleClick}>
            Clicked {count} times
        </button>
    );
}
```

This is already less verbose than the React counterpart, while achieving the exact same outcome.
There is also another significant difference: 

In UIX, the `MyButton` function is only executed once. All following DOM updates happen by direct
propagation of the `count` value.
This means that in contrast to React, the `handleClick` function and the returned button are only
ever created once and not from scratch each time the count value is updated.

You can also use functions like `setInterval` much more
intuitively in UIX.

```tsx
// UIX

function MyButton() {
    const count = $(0);

    // no hooks required!
    setInterval(() => count.val++, 1000);

    return (
        <div>
            Counter: {count}
        </div>
    );
}
```


## Sharing data between components

Similar to React, pointers and other values can be
passed to other components.

We are using the following React example as a base:

```tsx
// React

function MyButton({ count, onClick }) {
    return (
        <button onClick={onClick}>
            Clicked {count} times
        </button>
    );
}

export default function App() {
    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1);
    }

    return (
        <div>
            <h1>Counters that update together</h1>
            <MyButton count={count} onClick={handleClick} />
            <MyButton count={count} onClick={handleClick} />
        </div>
    );
}
```

The same behaviour can be achieved with UIX:

```tsx
// UIX

function MyButton({ count, onclick }) {
    return (
        <button onclick={onclick}>
            Clicked {count} times
        </button>
    );
}

export default function App() {
    const count = $(0);

    function handleClick() {
        count.val++;
    }

    return (
        <div>
            <h1>Counters that update together</h1>
            <MyButton count={count} onclick={handleClick} />
            <MyButton count={count} onclick={handleClick} />
        </div>
    );
}
```


## Conditional rendering

The following React example also works correctly with UIX:
Let's have a look at a simple React example that displays a different text
when a randomly generated value is greater or less than 1:

```tsx
// React

export function IsGreaterThan1({ value }) {
    return (
        <div>
            {value > 1 ? "Is greater than 1" : "Is less than or equal 1"}
        </div>
    );
}

export default function App() {
    const [random, setRandom] = useState(0);

    useEffect(() => {
        const interval = setInterval(() => {
            setRandom(Math.random() * 2);
        }, 1000);

        return () => {
            clearInterval(interval);
        };
    }, []);
    return (
        <div>
            Random value: {random}
            <IsGreaterThan1 value={random} />
        </div>
    );
}

```

The same result can be achieved with UIX in a much more compact way.
You can just directly use the setInterval function without wrapping it with useEffect.

```tsx
// UIX

export function IsGreaterThan1({ value }) {
    return (
        <div>
            {value > 1 ? "Is greater than 1" : "Is less than or equal 1"}
        </div>
    );
}

export default function App() {
    const random = $(0);

    setInterval(() => random.val = Math.random() * 2, 1000);

    return (
        <div>
            Random value: {random}
            <IsGreaterThan1 value={random} />
        </div>
    );
}
```

## Effects

This is a simple example for observing value changes in React with the `useEffect` hook:

```tsx
// React

export default () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log(`Count updated to: ${count}`);
    }, [count]); // Dependency array only watches `count`

    return (
        <div>
            <h1>Count: {count}</h1>
            <button onClick={() => setCount((prev) => prev + 1)}>
                Increment Count
            </button>
        </div>
    );
};
```

Similar to `useEffect` in React, you can use the `effect` function
in UIX. In contrast to React, you don't need to explicitly pass the
dependencies to the function - UIX detects all relevant dependencies automatically.

```tsx
// UIX

export default () => {
    const count = $(0);

    effect(() => {
        console.log(`Count updated to: ${count}`);
    });

    return (
        <div>
            <h1>Count: {count}</h1>
            <button onclick={() => count.val++}>
                Increment Count
            </button>
        </div>
    );
};
```
