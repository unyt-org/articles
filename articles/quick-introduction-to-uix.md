# A quick introduction to UIX

[UIX](https://github.com/unyt-org/uix), an open-source framework, isn't just an upgrade — it’s a evolution of how we think about full-stack development. With UIX, reactivity is automatic and seamless. Imagine building web apps where everything reacts dynamically without complex boilerplate. That’s what UIX brings to the table. ✨

If you haven’t tried UIX yet, you might want to consider it. Why? It combines front-end and back-end development into one cohesive experience, reducing the need for juggling multiple tools like compiling, REST APIs, Webpack, and SQL separately. Let’s dive in and see how UIX compares to traditional frameworks and why it’s a game-changer!

## Automatic Reactivity: No Boilerplate Needed
In frameworks like React or Vue, reactivity often involves complex setup. You need hooks, state management libraries, or boilerplate code to ensure everything updates when data changes. UIX removes all that friction — reactivity is baked in.

Let’s compare:

**React Example:**
```tsx
import { useState } from 'react';
function Counter() {
    const [count, setCount] = useState(0);
    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
            <p>{count}</p>
        </div>
    );
}
```
**UIX Example:**
```tsx
export function Counter() {
    const count = $(0);
    return (
        <div>
            <button onclick={() => count.val++}>
                Increment
            </button>
            <p>{count}</p>
        </div>
    );
};
```

UIX handles reactivity automatically. No `useState`, no `setState`, and no extra setup. The count variable updates the DOM as soon as its value changes. It’s simple, clean, and powerful.