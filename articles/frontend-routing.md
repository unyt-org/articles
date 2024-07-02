# Client-Side Routing for Web Apps with UIX

UIX leverages the power of the new [Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation) to seamlessly integrate client-side routing into the existing UIX routing system. This allows for a more fluid user experience by avoiding full page reloads and enabling smooth transitions between different views.

## What exactly is the Navigation API?

With the introduction of the Navigation API, web developers now have a standardized way to handle navigation events in the browser. This API provides a set of methods and events that allow developers to control the browser's history stack, navigate between pages, and handle user interactions such as back and forward button clicks.
This makes it possible to support routing for Single Page Applications (SPAs) without relying on third-party libraries or complex custom implementations.

Since the Navigation API is currently only supported by Chromium-based browsers, UIX uses a polyfill to ensure compatibility with other browsers as well.

## How does Routing work in UIX?

UIX renders content originating from the default exports of the `entrypoint.ts` modules.

The UIX project structure separates the frontend and backend code into different directories. The frontend code is located in the `frontend` directory, while the backend code resides in the `backend` directory.

Both of these directories can contain an `entrypoint.ts`.

When a user navigates to a specific route, UIX first tries to resolve the route server-side with the backend entrypoint. If the backend entrypoint cannot handle the route, UIX falls back to the frontend entrypoint to render the content.

To define a route in UIX, you can just add an entry to the object exported by the `entrypoint.ts` file. The key of the entry is the route path, and the value describes the content that should be rendered for that route.

Here is an example of a simple `entrypoint.ts` file:

```ts
export default {
  '/': () => <div>Welcome to the homepage!</div>,
  '/about': () => <div>Learn more about us!</div>,
  '/contact': () => <div>Contact us here!</div>,
}
```

When this entrypoint is located in the `backend` directory, the routes defined in this file will be resolved and rendered server-side. When the entrypoint is located in the `frontend` directory, the routes will be rendered client-side.
